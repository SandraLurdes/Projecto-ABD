# Tabelas do lakehouse (dicionário de dados)

Este documento lista as tabelas do schema `workspace.oil_psi`, camada a camada, e as suas colunas.
- O formato das fontes externas está em [FONTES.md](FONTES.md).
- O fluxo e as regras de cada camada estão em [ARQUITETURA.md](ARQUITETURA.md).

Os tipos vêm do código do notebook (casts, `StructType`, funções do Spark).
- Os marcados com `[FLAG]` são inferidos e não foram confirmados com `printSchema()`.
- `bool` = boolean, `str` = string, `ts` = timestamp.

| Camada | Tabela | Granularidade (chave) | Escrita | Partição | Criada por |
|---|---|---|---|---|---|
| Controlo | [`ctl_ingestion_log`](#ctl_ingestion_log) | série × leitura | append | — | `ingest` |
| Bronze | [`bz_fred_observations`](#bz_fred_observations) | série × data × leitura | append | `_ingest_date` | `load_bronze_fred` |
| Bronze | [`bz_yahoo_chart`](#bz_yahoo_chart) | símbolo × timestamp × leitura | append | `_ingest_date` | `load_bronze_yahoo` |
| Silver | [`sv_dim_instrument`](#sv_dim_instrument) | `instrument_id` | MERGE | — | célula 25 (`_dim_row`) |
| Silver | [`sv_fact_price_daily`](#sv_fact_price_daily-sv_fact_pt_shares_monthly-sv_fact_cpi_monthly) | `instrument_id`, `date` | MERGE | — | `build_silver` |
| Silver | [`sv_fact_pt_shares_monthly`](#sv_fact_price_daily-sv_fact_pt_shares_monthly-sv_fact_cpi_monthly) | `instrument_id`, `month_date` | MERGE | — | `build_silver` |
| Silver | [`sv_fact_cpi_monthly`](#sv_fact_price_daily-sv_fact_pt_shares_monthly-sv_fact_cpi_monthly) | `instrument_id`, `month_date` | MERGE | — | `build_silver` |
| Silver | [`sv_fact_equity_daily`](#sv_fact_equity_daily) | `instrument_id`, `date` | MERGE | — | `build_silver` |
| Gold | [`gd_market_daily`](#gd_market_daily) | `date` (dia útil) | overwrite | `year` + ZORDER(`date`) | `build_gold` |
| Gold | [`gd_market_monthly`](#gd_market_monthly) | `month_date` | overwrite | — | `build_gold` |
| Gold | [`gd_market_yearly`](#gd_market_yearly) | `year` | overwrite | — | `build_gold` |
| Gold | [`gd_equity_monthly`](#gd_equity_monthly) | `instrument_id`, `month_date` | overwrite | — | `build_gold_equity` |
| Gold | [`gd_oil_sensitivity`](#gd_oil_sensitivity) | `instrument_id`, `periodo` | overwrite | — | `build_gold_equity` |

A tabela `sv_fact_psi_daily` é de uma versão anterior e já não é criada. Está em `ALL_TABLES` só para o `RESET` a apagar.

---

## Controlo

### `ctl_ingestion_log`
Schema explícito em `LOG_SCHEMA` (célula 11). Tem uma linha por série e por leitura.

| Coluna | Tipo | Descrição |
|---|---|---|
| `batch_id` | str | `AAAAMMDDTHHMMSS_<6 hex>` (UTC) |
| `batch_name` | str | `carga_inicial`, `incremental` |
| `source` | str | `FRED` ou `YAHOO` |
| `series_id` | str | ID FRED ou símbolo Yahoo |
| `window_start`, `window_end` | str | Janela pedida (`AAAA-MM-DD`) |
| `extract_mode` | str | `API` ou `FALLBACK (<tipo de erro>)` |
| `records` | long | Linhas do CSV (sem o cabeçalho) ou nº de timestamps do JSON |
| `landing_file` | str | Caminho do ficheiro gravado na landing |
| `ingestion_ts` | ts | Hora da leitura (UTC) |

---

## Bronze
Os valores ficam **em bruto**. Todas as leituras são acumuladas (append-only). As colunas de linhagem são comuns às duas tabelas:

| Coluna | Tipo | Descrição |
|---|---|---|
| `_batch_id` | str | Leitura que trouxe a linha |
| `_ingestion_ts` | ts | `current_timestamp()` no momento da carga |
| `_source_file` | str | `_metadata.file_path`: o ficheiro da landing |
| `_ingest_date` | str | Extraída do caminho (`ingest_date=AAAA-MM-DD`); é a partição |

### `bz_fred_observations`
Uma linha por (série, data, leitura). Lida do CSV com `header=True`, por isso tudo chega como texto.

| Coluna | Tipo | Origem (FRED) |
|---|---|---|
| `series_id` | str | extraída do caminho `/fred/<série>/` |
| `observation_date` | str | `observation_date` |
| `value_raw` | str | coluna `<ID da série>`; pode ser `''` ou `.` |
| + linhagem | | |

### `bz_yahoo_chart`
Uma linha por (símbolo, timestamp, leitura). É obtida do JSON aninhado com `explode(chart.result)` + `posexplode(timestamp)`. Os arrays de `quote`/`adjclose` são indexados pela posição `i`.

| Coluna | Tipo | Origem (Yahoo) |
|---|---|---|
| `symbol` | str | `meta.symbol` |
| `currency` | str | `meta.currency` |
| `exchange_tz` | str | `meta.exchangeTimezoneName` |
| `ts_epoch` | long | `timestamp[i]` |
| `open_raw`, `high_raw`, `low_raw`, `close_raw` | double `[FLAG]` (inferido do JSON) | `indicators.quote[0].<campo>[i]` |
| `volume_raw` | long `[FLAG]` (inferido do JSON) | `indicators.quote[0].volume[i]` |
| `adjclose_raw` | double | `indicators.adjclose[0].adjclose[i]`; nulo se o bloco não existir |
| + linhagem | | |

---

## Silver
Colunas técnicas comuns aos factos:
- `dq_flag` (str, nulo se estiver OK);
- `_batch_id` (str);
- `_source_ingestion_ts` (ts, o `_ingestion_ts` da Bronze que venceu o dedupe);
- `_updated_ts` (ts, a hora da normalização).

### `sv_dim_instrument`
São 21 linhas, construídas a partir de `FRED_SERIES` + `YAHOO_SYMBOLS` (o schema é declarado em string na célula 25).

| Coluna | Tipo | Descrição |
|---|---|---|
| `instrument_id` | str | ID FRED ou símbolo Yahoo (chave) |
| `name` | str | Nome legível |
| `unit` | str | ex.: `USD/barril`, `Índice 2015=100`, `EUR`, `Pontos` |
| `currency` | str | `USD` / `EUR` |
| `frequency` | str | `D` / `M` |
| `category` | str | `Petróleo`, `Câmbio`, `Ações PT`, `Inflação` |
| `sector` | str | Setor da empresa (nulo nas séries que não são empresas e no PSI) |
| `oil_hypothesis` | str | Sinal da hipótese do grupo: `+` (Galp) / `-` (restantes) |
| `sector_view` | str | Sinal esperado por setor: `+` / `-` / `?` |
| `source` | str | `FRED` / `Yahoo Finance` |

### `sv_fact_price_daily`, `sv_fact_pt_shares_monthly`, `sv_fact_cpi_monthly`
Saem de `normalize_fred` e repartem-se pela frequência e pela categoria do instrumento:
- **diárias** (Brent, WTI, EUR/USD) → `sv_fact_price_daily`;
- **mensal de ações PT** (OCDE) → `sv_fact_pt_shares_monthly`;
- **mensal da inflação** (IHPC) → `sv_fact_cpi_monthly`.

| Coluna | Tipo | Descrição |
|---|---|---|
| `instrument_id` | str | ← `series_id` |
| `date` (diária) / `month_date` (mensais) | date | ← `observation_date` (`yyyy-MM-dd`); nas mensais é o dia 1 do mês |
| `value` | double | ← `value_raw`; `''`/`.` → nulo → linha eliminada |
| `dq_flag` | str | `PRECO_NAO_POSITIVO` se `value ≤ 0` |
| + técnicas | | |

### `sv_fact_equity_daily`
Sai de `normalize_yahoo` e tem o PSI e as 15 empresas.

| Coluna | Tipo | Descrição |
|---|---|---|
| `instrument_id` | str | ← `symbol` |
| `date` | date | ← `ts_epoch` convertido para a data de `Europe/Lisbon` |
| `open`, `high`, `low`, `close` | double | ← `*_raw`, arredondados a 4 casas |
| `adj_close` | double | ← `coalesce(adjclose_raw, close_raw)`, arredondado a 6 casas |
| `volume` | long | ← `volume_raw` |
| `dq_flag` | str | `PRECO_NAO_POSITIVO` (fecho ≤ 0) ou `MAX_MENOR_QUE_MIN` (máximo < mínimo) |
| + técnicas | | |

---

## Gold

### `gd_market_daily`
Tabela larga com um dia útil (seg–sex) por linha, desde 2000. É particionada por `year` e tem `OPTIMIZE ZORDER BY (date)`.

| Coluna | Tipo | Descrição |
|---|---|---|
| `date` | date | Calendário único (datas com petróleo/câmbio ∪ datas do PSI) |
| `brent_usd`, `wti_usd`, `eurusd`, `psi_close` | double | Com forward-fill a partir das observações da Silver |
| `psi_volume` | long | Volume do PSI (sem forward-fill) |
| `brent_quoted` | bool | Brent observado nesse dia (falso = forward-fill) |
| `psi_traded` | bool | PSI negociado nesse dia |
| `brent_eur` | double | `brent_usd / eurusd`, 4 casas |
| `ret_brent_usd`, `ret_brent_eur`, `ret_eurusd`, `ret_psi` | double | Retornos logarítmicos diários |
| `brent_eur_ma50`, `brent_eur_ma200`, `psi_ma50`, `psi_ma200` | double | Médias móveis de 50/200 dias |
| `corr_1y_psi_brent` | double | Correlação móvel de 252 dias entre `ret_psi` e `ret_brent_eur` (só com ≥ 200 observações) |
| `vol_1y_psi`, `vol_1y_brent` | double | Desvio-padrão de 252 dias × √252 |
| `psi_drawdown`, `brent_eur_drawdown` | double | Valor / máximo histórico até à data − 1 |
| `year`, `quarter`, `month` | int | Partição e filtros |

### `gd_market_monthly`
Uma linha por mês, desde 2000.

| Coluna | Tipo | Descrição |
|---|---|---|
| `month_date` | date | 1.º dia do mês |
| `brent_usd_avg`, `brent_eur_avg`, `wti_usd_avg` | double | Médias mensais (2 casas) |
| `eurusd_avg` | double | Média mensal (4 casas) |
| `brent_eur_eom`, `psi_eom` | double | Valor no último dia do mês |
| `psi_avg` | double | Média mensal do PSI |
| `pt_shares_idx` | double | Índice OCDE de ações PT (de `sv_fact_pt_shares_monthly`) |
| `ret_brent_eur_m`, `ret_pt_shares_m`, `ret_psi_m` | double | Retorno mensal **logarítmico** (`log(x / lag(x))`). Brent (`brent_eur_eom`) e PSI (`psi_eom`) no fim do mês; o índice OCDE só existe como média mensal (limitação da fonte). Antes de 13-11-2026 eram variações simples sobre médias |
| `yoy_brent_eur`, `yoy_pt_shares` | double | Variação face ao mesmo mês do ano anterior |
| `year` | int | |
| `hicp_publicado` | double | IHPC publicado (nulo nos meses ainda sem valor) |
| `hicp` | double | IHPC com forward-fill |
| `deflator` | double | Último `hicp` / `hicp` do mês |
| `brent_eur_real` | double | `brent_eur_avg × deflator`: euros a preços do último mês publicado |

### `gd_market_yearly`
Uma linha por ano.

| Coluna | Tipo | Descrição |
|---|---|---|
| `year` | int | |
| `brent_usd_avg`, `brent_usd_min`, `brent_usd_max` | double | |
| `brent_eur_avg`, `eurusd_avg` | double | |
| `brent_eur_eoy`, `psi_eoy` | double | Último valor do ano |
| `vol_brent_eur`, `vol_psi` | double | Volatilidade anualizada dos retornos diários |
| `corr_diaria_psi_brent` | double | Correlação diária `ret_psi` × `ret_brent_eur` no ano |
| `dias_psi` | long `[FLAG]` (soma de um int) | Dias de negociação do PSI |
| `pt_shares_eoy` | double | Índice OCDE no último mês do ano |
| `brent_eur_real_avg` | double | Média anual de `brent_eur_real` |
| `var_brent_eur`, `var_psi`, `var_pt_shares_oecd` | double | Fim de ano / fim do ano anterior − 1 |

### `gd_equity_monthly`
Uma linha por empresa × mês. Inclui o cabaz `PT_EX_GALP` (as outras 14 empresas, sem a Galp) como mais um instrumento.

| Coluna | Tipo | Descrição |
|---|---|---|
| `instrument_id`, `name`, `sector`, `oil_hypothesis`, `sector_view` | str | De `sv_dim_instrument` (+ a linha sintética do cabaz: setor `Cabaz sem Galp`, sinais `-`/`-`) |
| `month_date` | date | |
| `year` | int | |
| `dias_negociacao` | long | Dias com cotação no mês |
| `close_eom`, `adj_close_eom` | double | Fecho / fecho ajustado do último dia de negociação |
| `adj_close_real` | double | `adj_close_eom × deflator` |
| `ret_m` | double | Retorno logarítmico mensal com o fecho ajustado (só se o mês anterior for consecutivo). No cabaz é a média dos `ret_m_stat` |
| `ret_outlier` | bool | Mês com algum dia com \|variação\| > 40% (`OUTLIER_DAILY`) |
| `ret_m_stat` | double | `ret_m` sem os meses `ret_outlier`: é o retorno usado nas estatísticas |
| `ret_mercado_pt` | double | Média (pesos iguais) do `ret_m_stat` das **outras** empresas nesse mês |
| `brent_eur_eom` | double | Brent EUR no último dia do mês |
| `ret_brent_eur_m` | double | Retorno logarítmico mensal do Brent EUR |
| `ret_brent_eur_m_lag1..3` | double | O mesmo, 1, 2 e 3 meses antes |
| `ret_europa_m` | double | Retorno logarítmico mensal do Euro Stoxx 50 (`^STOXX50E`, fecho do último dia do mês); nulo antes de 2007-04 |
| `corr_36m_brent` | double | Correlação móvel de 36 meses `ret_m_stat` × `ret_brent_eur_m` (só com ≥ 30 pares) |

Desde 13-11-2026, `gd_market_monthly.ret_brent_eur_m` e `gd_equity_monthly.ret_brent_eur_m` têm a mesma definição (logarítmico, fim do mês).

### `gd_oil_sensitivity`
Uma linha por empresa (+ o cabaz) × período, com `PERIODS` = `Total 2000-2026`, `2000-2007`, `2008-2013`, `2014-2019` e `2020-2026`.
Cada mês entra no total e no seu subperíodo. As regressões são calculadas com agregações (variâncias e covariâncias por grupo).

| Coluna | Tipo | Descrição |
|---|---|---|
| `instrument_id`, `name`, `sector`, `oil_hypothesis`, `sector_view` | str | |
| `periodo` | str | |
| `ano_ini`, `ano_fim` | int | |
| `n_meses` | long | Meses com `ret_m_stat` e `ret_brent_eur_m` |
| `corr_m` | double | Correlação mensal com o Brent EUR |
| `corr_lag1..3` | double | Correlação com o Brent de 1, 2 e 3 meses antes |
| `beta`, `se_beta`, `t_beta` | double | `ret_ação ~ ret_brent_eur` (nulo se `n_meses < 24`) |
| `beta_sem_mercado`, `se_beta_sem_mercado`, `t_beta_sem_mercado` | double | Coeficiente do Brent em `ret_ação ~ ret_brent_eur + ret_mercado_pt` |
| `beta_mercado` | double | Coeficiente do mercado PT na mesma regressão |
| `n_meses_europa` | long | Meses com `ret_m_stat`, `ret_brent_eur_m` e `ret_europa_m` (2007-04+) |
| `beta_sem_europa`, `se_beta_sem_europa`, `t_beta_sem_europa` | double | Coeficiente do Brent em `ret_ação ~ ret_brent_eur + ret_europa_m` (efeito C; agregação à parte, só nos meses com Euro Stoxx 50) |
| `beta_europa` | double | Coeficiente da bolsa europeia na mesma regressão |
| `veredicto`, `veredicto_sem_mercado`, `veredicto_sem_europa` | str | `Confirma` / `Contraria` / `Sem efeito significativo` / `Dados insuficientes` (`_verdict`; regras em [METODOLOGIA.md](METODOLOGIA.md)) |

---

## Linhagem das colunas principais

| Conceito | Fonte | Bronze | Silver | Gold |
|---|---|---|---|---|
| Brent | `DCOILBRENTEU` (CSV) | `value_raw` | `sv_fact_price_daily.value` | `brent_usd` → `brent_eur` → `ret_brent_eur`, `ret_brent_eur_m` |
| Câmbio | `DEXUSEU` | `value_raw` | `value` | `eurusd` |
| Fecho ajustado | `indicators.adjclose[0].adjclose[i]` | `adjclose_raw` | `adj_close` | `adj_close_eom` → `ret_m` → `ret_m_stat` → `beta`, `corr_m` |
| PSI | `PSI20.LS` `quote.close[i]` | `close_raw` | `sv_fact_equity_daily.close` | `psi_close` → `ret_psi` |
| Ações PT (OCDE) | `SPASTT01PTM661N` | `value_raw` | `sv_fact_pt_shares_monthly.value` | `pt_shares_idx` → `ret_pt_shares_m` |
| Inflação | `CP0000PTM086NEST` | `value_raw` | `sv_fact_cpi_monthly.value` | `hicp_publicado` → `hicp` → `deflator` → `brent_eur_real`, `adj_close_real` |
| Data (Yahoo) | `timestamp[i]` (epoch UTC) | `ts_epoch` | `date` (Europe/Lisbon) | `date` / `month_date` |
