# CLAUDE.md — Projeto de grupo "Petróleo vs. Bolsa Portuguesa" (Analyzing Big Data, EDSA)

Este ficheiro dá o contexto do projeto. Lê-o todo antes de mexer em qualquer ficheiro desta pasta.

## 1. Quem e o quê

- **Unidade curricular:** Analyzing Big Data — EDSA (professor: ver o Moodle).
- **Grupo:** Ana Filipa Filipe 20250313, José Justino 20251110, Inês Rodrigues 20251957, Sandra Figueiredo 20251705.
- **Tema:** preço do petróleo (Brent/WTI) de 2000 até hoje vs. bolsa portuguesa (índice PSI e 15 empresas cotadas).
- **Hipótese do grupo:** *quando o preço do petróleo sobe, a cotação das empresas portuguesas desce — exceto a Galp* (para a Galp o petróleo é receita; para as outras é custo).
- **Língua:** português de Portugal (pt-PT) em todo o texto, nos comentários e nos nomes visíveis ao utilizador.
- **Entrega:** data a confirmar no Moodle. O roadmap do grupo propõe terminar a 16 de novembro de 2026.

## 2. Requisitos do professor (enunciado)

**Objetivo:** criar um processo de análise de dados estruturado e incremental que replique uma situação real.

**Requisitos**
- Criar um pequeno **Data Lakehouse** com um tema de análise e, no mínimo, uma fonte de dados.
- Usar **Apache Spark** no **Databricks Free Edition** ou no Google Colab (usamos o Databricks).
- Entregar o trabalho num **notebook em HTML** com um cabeçalho com o nome do grupo e dos elementos.

**Processo (ponta a ponta)**
1. Ler os dados de uma fonte, idealmente mais do que uma vez.
2. Criar uma camada com o histórico das leituras, pronta para reprocessamento (**Bronze**).
3. Normalizar os dados.
4. Guardar os dados normalizados e detalhados numa nova camada, modelada para o tema (**Silver**).
5. Preparar os dados para análise otimizada.
6. Criar uma nova camada com os dados otimizados para análise (**Gold**).
7. Fazer as análises: query, dashboard e modelo de ML.

**Conclusão:** resumir as conclusões sobre os dados, o processo e a tecnologia usada.

**Estilo da cadeira:** o notebook da aula 7 ("ABD07 The Data Lakehouse.html", na pasta-mãe) usa:
- uma arquitetura *medallion* com Volumes em `/Volumes/workspace/...` e um schema próprio;
- prefixos `BZ_` / `SV_` / `GD_` nos nomes das tabelas;
- "data rules" na Bronze/Silver (nulos, duplicados, tipos) e "business rules" na Silver.

Seguimos o mesmo espírito.

**Leituras recomendadas** (na pasta `Technical Resources-20260917/Documents`):
- Inmon, *Building the Data Lakehouse*, capítulos 1 e 6;
- *The Big Book of DW & BI*, secção 3.1;
- *Delta Lake: The Definitive Guide*.

## 3. Ficheiros desta pasta

| Ficheiro | O que é |
|---|---|
| `Lakehouse_Petroleo_PSI.ipynb` | **O entregável.** Notebook importado no Databricks (Workspace → Import). Versão de referência, já com os nomes do grupo. |
| `dados_fonte/` | 21 ficheiros de contingência (*seed*) com o formato exato das respostas das APIs: 5 `fred_<série>.csv` e 16 `yahoo_<símbolo>.json`. Snapshot de 07-11-2026. |
| `README.md` | Descrição do projeto para o grupo e para o professor. |
| `requirements.txt` | Bibliotecas usadas (já incluídas no Databricks; serve de documentação). |
| `CLAUDE.md` | Este ficheiro. |

No Databricks, os seeds vão para `/Volumes/workspace/oil_psi/lake/seed/`. O notebook também os encontra noutra pasta do Volume (função `find_seed`).

**Importante:** o grupo edita o notebook diretamente no Databricks (nomes, comentários). Antes de alterares o `.ipynb` desta pasta, pede uma exportação atualizada (*File → Export → IPython Notebook*) e trabalha sobre ela. Nunca apagues texto ou comentários escritos pelo grupo. Os comentários feitos com a ferramenta de comentários do Databricks não vêm no `.ipynb`.

## 4. Fontes de dados (gratuitas, sem chave)

| Fonte | Série / símbolo | Conteúdo | Freq. | Desde |
|---|---|---|---|---|
| FRED `https://fred.stlouisfed.org/graph/fredgraph.csv?id=<ID>&cosd=<início>&coed=<fim>` | `DCOILBRENTEU` | Brent spot, USD/barril | D | 1987 |
| FRED | `DCOILWTICO` | WTI spot, USD/barril (inclui −37,63 a 20-04-2020, valor real) | D | 1986 |
| FRED | `DEXUSEU` | USD por 1 EUR | D | 1999 |
| FRED / OCDE | `SPASTT01PTM661N` | Índice de preços de ações Portugal (2015=100) | M | 1988 |
| FRED / Eurostat | `CP0000PTM086NEST` | IHPC Portugal (inflação), usado para preços em euros de hoje | M | 1996 |
| Yahoo `https://query1.finance.yahoo.com/v8/finance/chart/<SÍMBOLO>?period1=<epoch>&period2=<epoch>&interval=1d` | `PSI20.LS` | Índice PSI (OHLCV) | D | **2013-04-30** |
| Yahoo | `GALP.LS EDP.LS EDPR.LS RENE.LS JMT.LS SON.LS IBS.LS BCP.LS NOS.LS CTT.LS EGL.LS NVG.LS ALTR.LS SEM.LS COR.LS` | Cotações das empresas (OHLCV) | D | 2000 ou IPO (Galp 2006, EDPR 2008, REN 2007, CTT 2013, Mota-Engil 2001) |

Notas sobre as fontes:
- No FRED, os valores em falta vêm como string vazia (ou `.`). Se o pedido levar várias séries com frequências diferentes, o FRED agrega para a frequência mais baixa, por isso pede-se uma série por pedido.
- O Yahoo precisa de um `User-Agent`. O JSON é aninhado: `chart.result[0].timestamp[]` e `indicators.quote[0].{open,high,low,close,volume}[]`. O `close` não está ajustado a dividendos; o fecho ajustado vem em `indicators.adjclose[0].adjclose[]` e é o usado nos retornos das empresas.
- **Não existe PSI diário gratuito antes de 2013.** A Euronext só dá 2 anos e o Stooq bloqueia. É por isso que se usam o índice mensal da OCDE e as empresas.
- O **Databricks Free Edition só permite internet para uma lista de domínios "confiáveis"**. O notebook tenta a API e, se falhar, usa o seed com a mesma janela de datas. O `ctl_ingestion_log.extract_mode` regista `API` ou `SEED (<erro>)`.

## 5. Arquitetura implementada no notebook

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

- **Schema:** `workspace.oil_psi`. **Volume:** `lake`.
- **Leituras incrementais:** 3 batches, `carga_inicial` (2000-01-01 → 2025-12-31), `incremental_1` (até 2026-06-30) e `incremental_2` (até hoje). Cada leitura parte do *watermark* da Bronze menos `OVERLAP_DAYS = 45`, o que gera duplicados de propósito; o `MERGE` na Silver resolve-os.
- **Pipeline incremental:** `run_pipeline()` faz API → Landing → Bronze → `build_silver()` → `build_gold()` → `build_gold_equity()`.
- **Reprocessamento:** `build_silver(batch_id=None)` reconstrói a Silver a partir de toda a Bronze. O notebook verifica que o resultado é idêntico.
- **`RESET = True`** apaga as tabelas e a landing (mas não o seed) e reconstrói tudo. É o modo usado para a entrega.
- `IS_DATABRICKS` deteta o ambiente. Fora do Databricks, as tabelas ficam em Parquet e o `MERGE` é substituído por um *anti-join + union* (permite correr em Colab ou Spark local).

## 6. Regras metodológicas (decisões do grupo)

- **A análise por empresa é mensal** (decisão de 07-11-2026): retornos logarítmicos com o fecho ajustado do último dia de negociação do mês. O dia a dia tem demasiado ruído.
- **As correlações, os betas e os testes fazem-se sempre sobre variações** (retornos logarítmicos), **nunca sobre níveis de preço**, por causa das correlações espúrias.
- O petróleo compara-se com o PSI e as empresas **em EUR**: `brent_eur = brent_usd / eurusd`.
- **Inflação:** o IHPC de Portugal só serve para os gráficos de evolução em euros de hoje (`brent_eur_real`, `adj_close_real`). As correlações usam retornos nominais. A paridade de poder de compra não se aplica (um só país, uma só moeda).
- **Setores e sinal esperado:** a hipótese testada é a do grupo (`oil_hypothesis`: + Galp, − restantes). A `sector_view` (+/−/?) é uma expectativa por setor, usada só para interpretar.
- **Significância:** |t| ≥ 2. O **veredicto** é "Confirma" se o sinal é o esperado (− para as empresas, + para a Galp) e é significativo, "Contraria" se é significativo com o sinal oposto, e "Sem efeito significativo" nos restantes casos.
- **Beta sem mercado:** regressão `ret_ação ~ ret_brent_eur + ret_mercado_PT`, em que o mercado PT é a média com pesos iguais das **outras** empresas nesse mês (sem a própria). Isola o efeito "custo do petróleo" do efeito "ciclo económico".
- **Qualidade:**
  - um dia com |variação| > 40% no fecho ajustado marca o mês com `ret_outlier` e esse mês fica fora das estatísticas (apanha 2 meses da Altri: 11/2000 e 03/2005);
  - preços ≤ 0 ficam com `dq_flag` e não são apagados.
- **ML:** divisão temporal (treino até 2023, teste de 2024 em diante), sem amostragem aleatória.

## 7. Resultados atuais (execução local de 07-11-2026, com os seeds, retornos mensais)

Teste local: PySpark 4.3 + Java 17, modo SEED forçado, tabelas em memória (o Windows sem `winutils.exe` não grava tabelas). As regressões do Spark foram conferidas com numpy (diferença < 1e-14).

- **Galp:** corr. mensal 0,42; +2,7% por cada +10% no Brent EUR (beta 0,27, t = 7,1); sem mercado +2,1% (t = 5,6). Estável em todos os subperíodos (0,43–0,52). **Confirma a hipótese.**
- **As outras 14 empresas:** todas com correlação positiva (0,01–0,27); 8 com beta positivo e significativo, nenhuma negativa significativa. Cabaz sem a Galp: +1,0% por +10% (t = 4,3). A hipótese **não se confirma** no efeito total (ciclo económico comum).
- **Sem o efeito do mercado:** 7/14 negativas (EDP, EDPR, REN, NOS, CTT, Jerónimo Martins, Sonae); significativas REN (−0,4%, t = −2,4) e CTT (−1,1%, t = −2,4). Exportadoras e BCP/Mota-Engil continuam positivas; Semapa e Ibersol positivas e significativas.
- **Ao longo do tempo:** correlação de 36 meses do cabaz sem Galp em 0,73 no fim de 2020 (crise de procura) e −0,32 em 11/2026 (choque de oferta, Brent EUR +83% em 2026); a Galp ficou em +0,50.
- **Desfasamentos:** correlações com o Brent de 1–3 meses antes entre −0,12 e 0,09 (nulas).
- **Inflação:** Brent em euros de hoje — 2008: 92 €; 2012: 115 €; 2022: 111 €; 2000: 56 €.
- **PSI vs Brent EUR:** corr. diária 0,21 e mensal 0,25. Em 2026 o Brent em EUR subiu 83% e o PSI 15%.
- **ML:** a regressão linear tem R² = 5%. O Random Forest (o PSI sobe amanhã?) tem accuracy 47,5% e AUC 0,44, abaixo da baseline de 53,4%.
- As conclusões (secção 7 do notebook) foram escritas com estes números. **Se os dados mudarem (por exemplo, se a API funcionar no Databricks), revê os números citados no texto.**

## 8. Estado e próximos passos

- [x] Fontes escolhidas e seeds descarregados (FRED + Yahoo, 21 ficheiros).
- [x] Notebook completo (Landing → Bronze → Silver → Gold → Análise → Conclusões), com análise mensal por empresa e inflação (07-11-2026), testado em Spark local.
- [ ] Fazer *upload* dos 21 seeds para `/Volumes/workspace/oil_psi/lake/seed/` (o grupo já tinha 5; faltam os 15 `yahoo_<empresa>.json` e o `fred_CP0000PTM086NEST.csv`).
- [ ] Correr **Run all** no Databricks e corrigir eventuais erros específicos do serverless.
- [ ] Criar o **AI/BI Dashboard** (secção 6.2: 5 widgets sobre as tabelas `gd_*`).
- [ ] Rever as conclusões com os números da execução no Databricks.
- [ ] Exportar o notebook em **HTML** (File → Export → HTML) e entregar no Moodle.

## 9. Notas técnicas para quem editar o notebook

- **Compute serverless do Free Edition:**
  - não usar `cache()`, `persist()` nem `spark.sparkContext`;
  - usar `_metadata.file_path` em vez de `input_file_name()`;
  - os Volumes estão acessíveis com `open()` / `os` do Python;
  - o MLlib é suportado (se falhar, a alternativa é o scikit-learn sobre `toPandas()`).
- **Células SQL:** o `.ipynb` usa células que começam por `%sql`.
- **Visualizações:** as que contam para o HTML são as de matplotlib, porque os gráficos do `display()` dependem de configuração na interface.
- **Testes fora do Databricks:**
  - o notebook corre em PySpark local sem Delta (testado com 4.3 + Java 17; o PySpark 4.0.x tem um erro nos *workers* Python em Windows);
  - em Windows, gravar tabelas exige `winutils.exe`; sem ele, testa-se com as tabelas em vistas temporárias;
  - os seeds têm de estar em `./lakehouse_oil_psi/seed/`;
  - as instruções `OPTIMIZE`, `DESCRIBE HISTORY` e `VERSION AS OF` só correm quando `TABLE_FORMAT == "delta"`.
- **Ao acrescentar uma série:**
  1. juntá-la a `FRED_SERIES` ou `YAHOO_SYMBOLS` (com `sector` e `hypothesis`);
  2. pôr o seed correspondente em `dados_fonte/`;
  3. atualizar a tabela de fontes no markdown do notebook.
