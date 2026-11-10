# Arquitetura e processo (medallion)

Este documento descreve **como os dados fluem**: camadas, regras aplicadas em cada uma, leituras
incrementais e reprocessamento.
- As colunas de cada tabela estão em [TABELAS.md](TABELAS.md).
- O formato das fontes está em [FONTES.md](FONTES.md).

Fonte de verdade: notebook, secções 0–5 (células 2, 5–6, 8, 16, 22, 28, 31, 35–42).

## 1. Visão geral

```
APIs ─► LANDING  /Volumes/workspace/oil_psi/lake/landing/<fonte>/<série>/ingest_date=AAAA-MM-DD/<série>_<batch_id>.csv|json
         │        (resposta em bruto, 1 ficheiro por leitura)
         ▼
BRONZE  bz_fred_observations, bz_yahoo_chart   (Delta, append-only, valores em bruto + _batch_id, _ingestion_ts,
         │                                       _source_file, _ingest_date; particionadas por _ingest_date)
         ▼  normalização: tipos, nulos ('' e '.'), dedupe pela leitura mais recente, data em Europe/Lisbon, dq_flag
SILVER  sv_dim_instrument (série, nome, setor, oil_hypothesis '+'/'-', sector_view '+'/'-'/'?'),
         │  sv_fact_price_daily (Brent/WTI/EURUSD), sv_fact_equity_daily (PSI + 15 empresas, OHLCV + adj_close),
         │  sv_fact_pt_shares_monthly (OCDE), sv_fact_cpi_monthly (IHPC)   → MERGE (upsert)
         ▼
GOLD    gd_market_daily (calendário único, forward-fill, Brent em EUR, retornos log, MA50/200, correlação móvel 252d,
         │                 volatilidade, drawdown; particionada por ano + OPTIMIZE ZORDER(date))
         │  gd_market_monthly (+ hicp, deflator, brent_eur_real), gd_market_yearly (+ brent_eur_real_avg)
         │  gd_equity_monthly (empresa × mês: retorno com fecho ajustado, ret_outlier, mercado PT sem a própria empresa,
         │                     Brent EUR e desfasamentos 1-3 meses, correlação móvel 36 meses, preço real; + cabaz PT_EX_GALP)
         │  gd_oil_sensitivity (empresa × período [Total 2000-2026 + 4 subperíodos]: corr mensal, beta, t, beta sem mercado,
         │                      veredictos, correlações desfasadas) — regressões calculadas com agregações (variâncias/covariâncias)
         ▼
ANÁLISE  Spark SQL (%sql), matplotlib, AI/BI Dashboard, MLlib (LinearRegression + RandomForestClassifier)
CONTROLO ctl_ingestion_log (uma linha por série e por leitura)
```

- **Catálogo / schema:** `workspace.oil_psi`. **Volume:** `lake` (`/Volumes/workspace/oil_psi/lake`), com as pastas `landing/` e `seed/`.
- **Prefixos:** `ctl_` (controlo), `bz_` (Bronze), `sv_` (Silver), `gd_` (Gold). É o estilo do notebook da aula 7
  ("ABD07 The Data Lakehouse"), que usa `BZ_` / `SV_` / `GD_`.

## 2. Camadas

| Camada | O que guarda | Como escreve | Função |
|---|---|---|---|
| **Landing** | A resposta da API (ou do seed) **sem qualquer alteração**, 1 ficheiro por série e por leitura | ficheiro novo | `land` (chamada por `ingest`) |
| **Controlo** | Uma linha por série e por leitura (janela, modo API/SEED, nº de registos, ficheiro) | append | `ingest` |
| **Bronze** | **Todas** as leituras, sem descartar nada; valores em bruto + metadados de linhagem | append, particionada por `_ingest_date` | `load_bronze` = `load_bronze_fred` + `load_bronze_yahoo` |
| **Silver** | Dados normalizados e detalhados, modelados para o tema: 1 dimensão + 4 factos | `MERGE` (upsert) pelas chaves de negócio | `upsert`, `normalize_fred`, `normalize_yahoo`, `build_silver` |
| **Gold** | Tabelas largas (desnormalizadas) prontas para análise, sem joins na hora da consulta | overwrite (`overwriteSchema`) | `build_gold`, `build_gold_equity` |

### Data rules (Bronze → Silver)
1. **Tipos:** `value_raw` (texto) → `double`; datas em texto → `date`; *timestamps* Unix → data local de Lisboa (`Europe/Lisbon`).
2. **Nulos:** os valores vazios ou `.` são eliminados. No PSI e nas empresas, os dias sem fecho são eliminados.
3. **Duplicados:** a mesma (série, data) pode chegar em várias leituras. Fica sempre a **leitura mais recente**
   (`latest_per_key`: ordena por `_ingestion_ts` desc e `_batch_id` desc), o que também corrige revisões da fonte.
4. **Validação:** os preços ≤ 0 ficam com `dq_flag = 'PRECO_NAO_POSITIVO'` e não são apagados (ex.: WTI a −37,63 USD a
   20-04-2020). Nas ações também se marca `MAX_MENOR_QUE_MIN`.
5. **Ações:** guarda-se o fecho e o **fecho ajustado** (`adj_close`, que usa o `close` quando falta o ajustado).
   Os preços ficam com 4 casas decimais (há ações abaixo de 1 €, como o BCP) e o `adj_close` fica com 6.

A qualidade em bruto é medida antes de normalizar (notebook 2.1): valores em falta, duplicados, fecho e ajustado em falta, volume zero.

### Business rules (Silver → Gold)
- **Calendário único** de dias úteis (seg–sex): a união das datas com cotação do petróleo/câmbio e do PSI.
- **Forward-fill** dos preços nos feriados de um dos mercados, com indicadores `brent_quoted` / `psi_traded`.
- **Brent em EUR** = `brent_usd / eurusd`.
- **Retornos logarítmicos**, médias móveis de 50 e 200 dias, correlação móvel de 252 dias (só com ≥ 200 observações),
  volatilidade anualizada (×√252), *drawdowns*.
- **Inflação:** o IHPC, com forward-fill nos meses ainda sem valor publicado, dá o `deflator` (o último IHPC a dividir pelo IHPC do mês)
  e daí os preços em euros de hoje.
- **Empresas** (secção 4.1): análise mensal com o fecho ajustado do último dia de negociação, `ret_outlier`, mercado PT sem a própria
  empresa, cabaz `PT_EX_GALP` e regressões por empresa × período. As regras estatísticas estão em [METODOLOGIA.md](METODOLOGIA.md).

## 3. Leituras incrementais

São 3 leituras (*batches*) das 21 séries:

| Batch | Janela | Onde |
|---|---|---|
| `carga_inicial` | 2000-01-01 → 2025-12-31 (`HIST_START`, `HIST_END`) | notebook 1.1, `ingest` |
| `incremental_1` | *watermark* − 45 dias → 2026-06-30 | notebook 5, `run_pipeline` |
| `incremental_2` | *watermark* − 45 dias → hoje | notebook 5, `run_pipeline` |

- **Watermark** (`watermarks()`): a última data por série na Bronze (`max(observation_date)` no FRED e `max(ts_epoch)` no Yahoo).
- **`OVERLAP_DAYS = 45`:** cada leitura recomeça 45 dias antes do watermark. Isto apanha as correções da fonte, mas gera **duplicados
  de propósito** na Bronze (65 por série diária na execução de 07-11-2026), que o `MERGE` da Silver resolve.
- **`batch_id`** = `AAAAMMDDTHHMMSS_<6 hex>` (UTC).
- **`run_pipeline(batch_name, end_date)`** faz API → Landing → Bronze → `build_silver(bid)` → `build_gold()` → `build_gold_equity()`.

## 4. Upsert e reprocessamento

- **`upsert(df, table, keys)`:** se a tabela não existir, cria-a. Em Delta faz `MERGE INTO … WHEN MATCHED UPDATE SET * WHEN NOT MATCHED INSERT *`.
  Fora do Databricks (Parquet) faz *anti-join* + `union` + overwrite.
- **`build_silver(batch_id=None)`** reconstrói a Silver a partir de **toda** a Bronze. O notebook (5.1) apaga as 4 tabelas de factos
  (`SILVER_FACTS`), reconstrói-as e verifica que as contagens são idênticas (`Silver idêntica após reprocessamento: True`).
- **`RESET = True`** apaga todas as tabelas de `ALL_TABLES` e a landing (o seed **não** é apagado) e reconstrói tudo. É o modo usado para
  a entrega (em produção ficaria `False`). Também apaga `sv_fact_psi_daily`, uma tabela de uma versão anterior (o PSI passou para
  `sv_fact_equity_daily`).

## 5. Delta Lake e otimização

- Transações ACID, `MERGE` para upserts.
- **Time travel:** `DESCRIBE HISTORY gd_market_daily` e `SELECT … VERSION AS OF 0` (notebook 5, célula 42).
- **`gd_market_daily`** está particionada por `year` e tem `OPTIMIZE … ZORDER BY (date)`, para compactar ficheiros pequenos e fazer *data skipping*.
- A Bronze está particionada por `_ingest_date`.
- As instruções `OPTIMIZE`, `DESCRIBE HISTORY` e `VERSION AS OF` só correm quando `TABLE_FORMAT == "delta"`.

## 6. Ambiente (`IS_DATABRICKS`)

- `IS_DATABRICKS = "DATABRICKS_RUNTIME_VERSION" in os.environ`.
- **No Databricks:** Delta + Unity Catalog, `BASE = /Volumes/workspace/oil_psi/lake`.
- **Fora do Databricks** (Colab / Spark local):
  - as tabelas ficam em Parquet em `./lakehouse_oil_psi/warehouse/` e `BASE = ./lakehouse_oil_psi`;
  - o catálogo é em memória, por isso o `RESET` apaga também as pastas Parquet das tabelas (o `DROP TABLE` não as encontra num kernel
    novo);
  - o `MERGE` é substituído por *anti-join + union*;
  - o notebook define um `display()` simples;
  - o notebook prepara sozinho o ambiente: `PYSPARK_PYTHON`, deteção do `JAVA_HOME` (macOS/Windows) e uma *cell magic* `sql` com
    um *input transformer* que faz correr as células `%sql` com `spark.sql(...)`.
- **Nos dois ambientes**, a célula do schema copia os seeds de `dados_fonte/` (pasta do notebook ou a pasta-mãe) para `SEED`, de forma
  idempotente: só copia os que faltam ou têm tamanho diferente. No Databricks isto funciona com o repositório numa Git folder, onde a
  pasta de trabalho é a do notebook. Sem `dados_fonte/`, o *upload* para o Volume é manual.

  Ver [DESENVOLVIMENTO.md](DESENVOLVIMENTO.md).

## 7. Mapa do notebook (funções por secção)

As funções são definidas na mesma célula em que são usadas pela primeira vez. Por isso o notebook só corre **de cima para baixo** (Run all).

| Secção | Conteúdo | Funções / constantes |
|---|---|---|
| Cabeçalho | Título, grupo, objetivo, fontes, arquitetura, empresas | — |
| 0 Configuração | Ambiente, schema, Volume, reset, catálogo de séries | `IS_DATABRICKS`, `TABLE_FORMAT`, `BASE`/`LANDING`/`SEED`, `RESET`, `ALL_TABLES`, `FRED_SERIES`, `YAHOO_SYMBOLS`, `_eq` |
| 1 Ingestão / Landing | Pedidos às APIs, contingência, gravação na landing, log | `HTTP_HEADERS`, `_http_get`, `_to_epoch`, `fetch_fred`, `fetch_yahoo`, `find_seed`, `land`, `LOG_SCHEMA`, `ingest` |
| 1.1 1.ª leitura | Carga histórica | `HIST_START`, `HIST_END`, `all_series` |
| 2 Bronze (+ 2.1 qualidade) | Carga append | `load_bronze_fred`, `load_bronze_yahoo`, `load_bronze` |
| 3 Silver | Normalização + MERGE | `upsert`, `latest_per_key`, `_dim_row`, `normalize_fred`, `normalize_yahoo`, `SILVER_FACTS`, `build_silver` |
| 4 Gold | Mercado diário / mensal / anual | `build_gold` |
| 4.1 Gold empresas | Retornos mensais, sensibilidade | `OUTLIER_DAILY`, `MIN_MONTHS`, `PERIODS`, `BASKET_ID`, `_verdict`, `build_gold_equity` |
| 5 Incrementais (+ 5.1 reprocessamento) | 2 leituras, log, comparação Bronze vs Silver, time travel | `OVERLAP_DAYS`, `watermarks`, `run_pipeline` |
| 6 Análise | Queries, gráficos, dashboard, ML | ver [ANALISES.md](ANALISES.md) |
| 7 Conclusões | Texto | ver [CONCLUSOES.md](CONCLUSOES.md) |
