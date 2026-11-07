# Petróleo vs. Bolsa Portuguesa (2000–2026): um Data Lakehouse com Apache Spark

Projeto de grupo da unidade curricular **Analyzing Big Data** (EDSA).

**Elementos do grupo**
- Ana Filipa Filipe — 20250313
- José Justino — 20251110
- Inês Rodrigues — 20251957
- Sandra Figueiredo — 20251705

**Plataforma:** Apache Spark no **Databricks Free Edition** (compute serverless, Unity Catalog, Delta Lake)

---

## Objetivo

Criar um processo de análise de dados **estruturado e incremental**, que replique uma situação real, para estudar a relação entre o **preço do petróleo** (Brent e WTI) e a **bolsa portuguesa** (índice PSI e 15 empresas cotadas), de 2000 até hoje.

### Hipótese do grupo
> **Quando o preço do petróleo sobe, a cotação das empresas portuguesas desce — exceto a Galp.**
> Para a Galp o petróleo é receita; para as outras empresas é um custo.

### Perguntas de análise
1. Como evoluiu o preço do petróleo desde 2000, em dólares, em euros e em euros a preços de hoje (descontando a inflação)?
2. O mercado acionista português acompanha o petróleo? Em que períodos a relação é mais forte?
3. Como reage cada empresa ao petróleo, mês a mês? A Galp comporta-se de forma diferente das restantes? E depois de descontar o movimento geral da bolsa?
4. Como se comportaram os mercados nas grandes crises (2008, 2014-16, COVID-19, guerra na Ucrânia)?
5. Os movimentos do petróleo ajudam a explicar ou a prever os movimentos do PSI?

---

## Estrutura do repositório

```
Projecto-ABD/
├── Lakehouse_Petroleo_PSI.ipynb   # entregável: notebook importado no Databricks
├── dados_fonte/                   # 21 ficheiros de contingência (seed) com o formato das APIs (snapshot de 07-11-2026)
│   ├── fred_<série>.csv           # 5 séries FRED (Brent, WTI, EUR/USD, índice OCDE ações PT, IHPC Portugal)
│   └── yahoo_<símbolo>.json       # 16 séries Yahoo (PSI + 15 empresas)
├── CLAUDE.md                      # contexto técnico e decisões do grupo
├── requirements.txt               # bibliotecas usadas (já incluídas no Databricks)
└── README.md
```

---

## Fontes de dados (gratuitas, sem chave)

| Fonte | Série / símbolo | Conteúdo | Freq. | Desde |
|---|---|---|---|---|
| **FRED** (Federal Reserve Bank of St. Louis) | `DCOILBRENTEU` | Brent spot (USD/barril) | Diária | 1987 |
| FRED | `DCOILWTICO` | WTI spot (USD/barril) | Diária | 1986 |
| FRED | `DEXUSEU` | Câmbio USD por 1 EUR | Diária | 1999 |
| FRED / OCDE | `SPASTT01PTM661N` | Índice de preços de ações — Portugal (2015 = 100) | Mensal | 1988 |
| FRED / Eurostat | `CP0000PTM086NEST` | IHPC Portugal — índice harmonizado de preços no consumidor (inflação) | Mensal | 1996 |
| **Yahoo Finance** (chart API) | `PSI20.LS` | Índice PSI (OHLCV) | Diária | 2013-04-30 |
| Yahoo Finance | `GALP` `EDP` `EDPR` `RENE` `JMT` `SON` `IBS` `BCP` `NOS` `CTT` `EGL` `NVG` `ALTR` `SEM` `COR` (`.LS`) | Cotações das 15 empresas (OHLCV + fecho ajustado a dividendos) | Diária | 2000 ou data de entrada em bolsa |

Notas:
- **Não existe PSI diário gratuito antes de 2013.** Por isso usam-se também o índice mensal da OCDE e as cotações das empresas.
- Investing.com e Euronext foram considerados: o primeiro não tem API e proíbe a recolha automática; o segundo só dá 2 anos de histórico gratuito.
- O Databricks Free Edition só permite acesso à internet para domínios "confiáveis". O notebook tenta a API e, se falhar, usa o ficheiro *seed* com a mesma janela de datas. A tabela `ctl_ingestion_log` regista o modo (`API` ou `SEED`).

---

## Arquitetura (Medallion)

```
APIs ─► LANDING  /Volumes/workspace/oil_psi/lake/landing/...   (resposta em bruto, 1 ficheiro por leitura)
         │
         ▼
BRONZE   bz_fred_observations, bz_yahoo_chart   (Delta, append-only, valores em bruto + metadados de linhagem)
         │  normalização: tipos, nulos, duplicados (leitura mais recente), datas em Europe/Lisbon, dq_flag
         ▼
SILVER   sv_dim_instrument, sv_fact_price_daily, sv_fact_equity_daily,
         │  sv_fact_pt_shares_monthly, sv_fact_cpi_monthly                       (MERGE/upsert)
         │  integração, forward-fill, Brent em EUR, inflação, retornos, médias móveis, betas
         ▼
GOLD     gd_market_daily, gd_market_monthly, gd_market_yearly, gd_equity_monthly, gd_oil_sensitivity
         │
         ▼
ANÁLISE  Spark SQL, matplotlib, AI/BI Dashboard, Spark MLlib
```

### Tabelas (schema `workspace.oil_psi`)

| Camada | Tabela | Descrição |
|---|---|---|
| Controlo | `ctl_ingestion_log` | Uma linha por série e por leitura (janela, modo API/SEED, nº de registos) |
| Bronze | `bz_fred_observations` | Observações FRED em bruto, particionadas por `_ingest_date` |
| Bronze | `bz_yahoo_chart` | Cotações Yahoo em bruto (incluindo o fecho ajustado), extraídas do JSON aninhado |
| Silver | `sv_dim_instrument` | Dimensão de instrumentos (nome, setor, moeda, fonte, sinal esperado pela hipótese e pela leitura do setor) |
| Silver | `sv_fact_price_daily` | Factos diários: Brent, WTI, EUR/USD |
| Silver | `sv_fact_equity_daily` | Factos diários: PSI + 15 empresas (OHLCV + fecho ajustado) |
| Silver | `sv_fact_pt_shares_monthly` | Índice mensal OCDE de ações portuguesas |
| Silver | `sv_fact_cpi_monthly` | IHPC mensal de Portugal (inflação) |
| Gold | `gd_market_daily` | Tabela diária larga: Brent em EUR, retornos log, MA50/200, correlação e volatilidade móveis, drawdowns; particionada por ano + `OPTIMIZE ZORDER BY (date)` |
| Gold | `gd_market_monthly` / `gd_market_yearly` | Agregados mensais e anuais, incluindo o Brent em euros a preços de hoje |
| Gold | `gd_equity_monthly` | Uma linha por empresa e mês: retorno mensal (fecho ajustado), mercado PT sem a própria empresa, retorno do Brent (EUR) e desfasamentos, correlação móvel de 36 meses, preço real; inclui o cabaz `PT_EX_GALP` |
| Gold | `gd_oil_sensitivity` | Por empresa e período: correlação, beta, beta sem mercado, estatísticas t, veredicto sobre a hipótese e correlações desfasadas |

### Características do processo
- **Incremental:** 3 leituras de 21 séries — `carga_inicial` (2000-01-01 → 2025-12-31), `incremental_1` (até 2026-06-30) e `incremental_2` (até hoje). Cada leitura parte do *watermark* da Bronze com 45 dias de sobreposição; o `MERGE` na Silver resolve os duplicados.
- **Reprocessável:** a Silver é reconstruída de raiz a partir da Bronze e o notebook verifica que o resultado é idêntico.
- **Linhagem:** cada registo guarda `_batch_id`, `_ingestion_ts`, `_source_file` e `_ingest_date`.
- **Delta Lake:** transações ACID, `MERGE`, *time travel* (`DESCRIBE HISTORY`, `VERSION AS OF`), `OPTIMIZE … ZORDER BY`.

---

## Regras metodológicas

- A análise por empresa é **mensal**: retornos logarítmicos com o fecho ajustado do último dia de negociação de cada mês.
- Correlações, betas e testes são calculados **sempre sobre retornos**, **nunca sobre níveis de preço**, para evitar correlações espúrias.
- O petróleo é comparado com o PSI e com as empresas **em euros**: `brent_eur = brent_usd / eurusd`.
- **Inflação:** o IHPC de Portugal serve para mostrar os preços em euros de hoje nos gráficos de evolução. As correlações usam retornos nominais, porque a inflação mensal (~0,2%) quase não as altera.
- **Significância:** |t| ≥ 2. **Veredicto:** "Confirma" se o sinal é o esperado (− para as empresas, + para a Galp) e significativo; "Contraria" se é significativo com o sinal oposto; "Sem efeito significativo" nos restantes casos.
- **Beta sem mercado:** `ret_ação ~ ret_brent_eur + ret_mercado_PT`, em que o mercado PT é a média com pesos iguais das **outras** empresas. Separa o efeito "custo do petróleo" do efeito "ciclo económico".
- **Qualidade:** um dia com variação > 40% no fecho ajustado marca o mês com `ret_outlier`, e esse mês fica fora das estatísticas (2 meses da Altri). Preços ≤ 0 ficam com `dq_flag` e não são apagados (ex.: WTI a −37,63 USD em 20-04-2020).
- **ML:** divisão temporal (treino até 2023, teste de 2024 em diante), sem amostragem aleatória.

---

## Como executar (Databricks Free Edition)

As bibliotecas usadas (`pyspark`, `requests`, `pandas`, `numpy`, `matplotlib`) já vêm instaladas no Databricks; o [requirements.txt](requirements.txt) serve apenas como documentação.

1. Importar `Lakehouse_Petroleo_PSI.ipynb` para o workspace (*Workspace → Import*).
2. Correr as células da secção **0. Configuração do ambiente** (criam o schema `workspace.oil_psi` e o Volume `lake`).
3. Em **Catalog → workspace → oil_psi → Volumes → lake**, criar a pasta `seed` e fazer *upload* dos **21 ficheiros** de `dados_fonte/`.
4. Correr **Run all**. Com `RESET = True` todas as tabelas e a landing são apagadas e reconstruídas (o seed é mantido).
5. Criar o **AI/BI Dashboard** (*New → Dashboard*) sobre as tabelas `gd_*` (widgets propostos na secção 6.2 do notebook).
6. Exportar o notebook em **HTML** (*File → Export → HTML*) para a entrega.

---

## Principais resultados

_Execução local de 07-11-2026 com os ficheiros seed (PySpark 4.3, modo SEED). Os números devem ser confirmados depois da execução no Databricks._

**Petróleo**
- Em euros a preços de hoje, a média de 2008 (92 €/barril) foi mais baixa do que as de 2012 (115 €) e de 2022 (111 €): o euro forte amorteceu o choque de 2008.

**Hipótese do grupo (retornos mensais, 2000–2026)**
- **Galp — confirma.** Correlação mensal 0,42; num mês em que o Brent (EUR) sobe 10%, a Galp sobe em média +2,7% (t = 7,1), e +2,1% descontando a bolsa (t = 5,6).
- **As outras 14 — não confirmam no efeito total.** Todas têm correlação positiva com o petróleo (0,01 a 0,27) e 8 sobem de forma significativa com ele. O cabaz sem a Galp sobe +1,0% por cada +10% no Brent (t = 4,3). Petróleo e bolsa seguem o mesmo ciclo económico.
- **Descontando o movimento da bolsa:** 7 das 14 ficam negativas (elétricas, NOS, CTT, Jerónimo Martins, Sonae). A REN e os CTT confirmam a hipótese de forma significativa. As exportadoras e as empresas com negócio em países petrolíferos (BCP, Mota-Engil) continuam positivas.
- **A relação depende do tipo de choque:** nas crises de procura (2020–22) tudo se move junto; no choque de 2025–26 a correlação de 36 meses do cabaz sem a Galp caiu para −0,32, enquanto a Galp ficou em +0,50.
- **Sem desfasamento:** o petróleo de 1–3 meses antes não explica a bolsa de hoje.

**PSI e ML**
- PSI vs Brent EUR: correlação diária 0,21 e mensal 0,25.
- A regressão linear explica só 5% (R²) da variação diária do PSI. O Random Forest (o PSI sobe amanhã?) teve accuracy de 47,5% e AUC de 0,44, abaixo da regra ingénua "sobe sempre" (53,4%).

---

## Estado do projeto

- [x] Fontes escolhidas e seeds descarregados (FRED + Yahoo, 21 ficheiros)
- [x] Notebook completo (Landing → Bronze → Silver → Gold → Análise → Conclusões), com análise por empresa e inflação, testado em Spark local
- [ ] *Upload* dos 21 seeds para `/Volumes/workspace/oil_psi/lake/seed/`
- [ ] **Run all** no Databricks e correção de erros específicos do serverless
- [ ] AI/BI Dashboard
- [ ] Rever as conclusões com os números da execução no Databricks
- [ ] Exportar em HTML e entregar no Moodle (até 16-11-2026, 23:59)
- [ ] Inscrever o grupo num horário de apresentação no Moodle (17 ou 19-11-2026, 15 minutos)
