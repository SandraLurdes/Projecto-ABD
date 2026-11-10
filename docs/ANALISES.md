# Catálogo de análises (secção 6 do notebook)

Este documento indica que análises existem, a que pergunta responde cada uma e que tabelas `gd_*` lê.
- As colunas das tabelas estão em [TABELAS.md](TABELAS.md).
- Os resultados estão em [CONCLUSOES.md](CONCLUSOES.md).

As perguntas P1–P5 estão em [METODOLOGIA.md](METODOLOGIA.md#1-hipótese-do-grupo).

## 6.1 Queries

Todas são `%sql`, exceto a de correlações, que é PySpark.

| # | Célula | Análise | Pergunta | Tabelas |
|---|---|---|---|---|
| Q1 | 46 | **Resumo anual:** Brent médio/mín/máx em USD, em EUR e em EUR a preços de hoje; variação anual do Brent EUR, do PSI e do índice OCDE; volatilidade e correlação diária PSI–Brent | P1, P2 | `gd_market_yearly` |
| Q2 | 48 | **Grandes crises:** variação do Brent EUR, das ações PT (OCDE) e do PSI em 9 episódios (dot-com, petróleo 2007-08, crise de 2008, troika, colapso de 2014-16, COVID-19, recuperação, Ucrânia, 2026) | P4 | `gd_market_monthly` |
| Q3 | 50 | **10 dias com maior variação do Brent** (EUR) desde 2013 e o que o PSI fez nesse dia | P2 | `gd_market_daily` |
| Q4 | 52 | **Regimes do petróleo:** retorno médio mensal das ações PT e % de meses em que sobem, conforme o Brent cai > 10%, cai 3–10%, fica estável (±3%), sobe 3–10% ou sobe > 10% | P2 | `gd_market_monthly` |
| Q5 | 54 (PySpark) | **Correlações por frequência:** diária PSI vs Brent EUR e PSI vs EUR/USD (2013–2026); mensal OCDE vs Brent (2000–2026); mensal PSI vs Brent (2013–2026); níveis mensais OCDE vs Brent | P2 | `gd_market_daily`, `gd_market_monthly` |
| Q6 | 56 | **Hipótese por empresa** (total 2000-2026): corr. mensal, efeito total e efeito sem mercado por +10% no Brent, t, veredictos | P3 | `gd_oil_sensitivity` |
| Q7 | 58 | **Por setor:** médias dos efeitos e contagem de *Confirma* / *Contraria*, antes e depois de descontar a bolsa | P3 | `gd_oil_sensitivity` |
| Q8 | 60 | **Ao longo do tempo:** corr. mensal por subperíodo (pivot por empresa) | P2, P3 | `gd_oil_sensitivity` |
| Q9 | 62 | **Desfasamentos:** correlação com o Brent no mesmo mês e 1, 2 e 3 meses antes | P3, P5 | `gd_oil_sensitivity` |

As células `%sql` não correm fora do Databricks. Ver [DESENVOLVIMENTO.md](DESENVOLVIMENTO.md).

## 6.2 Gráficos (matplotlib)

Os gráficos que contam para o HTML entregue são os de matplotlib, porque os do `display()` dependem de configuração na interface.

| # | Célula | Gráfico | Tabelas |
|---|---|---|---|
| G1 | 65 | Painel 2×2: Brent mensal USD vs EUR; ações PT (OCDE) vs Brent EUR (2 eixos); PSI vs Brent EUR em base 100 (2013+); correlação móvel de 1 ano PSI–Brent | `gd_market_monthly`, `gd_market_daily` |
| G2 | 67 | Dispersão dos retornos mensais Brent EUR × ações PT; barras da variação anual Brent EUR vs OCDE | `gd_market_monthly`, `gd_market_yearly` |
| G3 | 70 | Brent em EUR nominal vs real, com anotações de 2008, 2012 e 2022; tabela de anos selecionados. Esta célula define também o estilo comum (paleta para daltonismo, `SECTOR_COLORS`, `CRISES`) | `gd_market_monthly`, `gd_market_yearly` |
| G4 | 72 | Mapa de calor: correlação móvel de 36 meses de cada empresa com o Brent, no fim de cada ano, agrupada por setor | `gd_equity_monthly` |
| G5 | 74 | Barras: efeito total vs efeito sem mercado por empresa, com barras de erro de ±2 erros-padrão, coloridas por setor | `gd_oil_sensitivity` |
| G6 | 76 | Linhas: correlação móvel de 36 meses da Galp vs o cabaz sem Galp, com as crises sombreadas | `gd_equity_monthly` |
| G7 | 78 | Base 100 a preços de hoje desde a entrada da Galp em bolsa: Galp, cabaz sem Galp e Brent EUR | `gd_equity_monthly`, `gd_market_monthly` |

## 6.2 AI/BI Dashboard (por criar no Databricks)

Cria-se em *New → Dashboard*, com as fontes de dados `gd_market_daily`, `gd_market_monthly`, `gd_market_yearly`, `gd_equity_monthly` e
`gd_oil_sensitivity`. Os widgets propostos no notebook (célula 64) são:

| # | Widget | Tipo | Tabela | Campos sugeridos |
|---|---|---|---|---|
| W1 | Brent mensal 2000–2026 em USD, em EUR e em EUR a preços de hoje | linhas | `gd_market_monthly` | `month_date`; `brent_usd_avg`, `brent_eur_avg`, `brent_eur_real` |
| W2 | Correlação móvel de 1 ano PSI–Brent | linha | `gd_market_daily` | `date`; `corr_1y_psi_brent` |
| W3 | Efeito do petróleo por empresa, descontando a bolsa | barras coloridas por setor | `gd_oil_sensitivity` | `name`; `beta_sem_mercado × 10`; cor = `sector`; filtro `periodo = 'Total 2000-2026'` |
| W4 | Correlação móvel de 36 meses com o Brent, com filtro por empresa | linhas | `gd_equity_monthly` | `month_date`; `corr_36m_brent`; filtro = `name` |
| W5 | Tabela de resumo anual | tabela | `gd_market_yearly` | as colunas de Q1 |

A coluna "Campos sugeridos" mapeia cada widget para colunas existentes. O notebook só descreve o widget.

## 6.3 Modelos de Machine Learning (Spark MLlib)

| Modelo | Célula | Objetivo | Dados | Variáveis | Avaliação |
|---|---|---|---|---|---|
| **Regressão linear** (explicativo) | 67 | Quanto do retorno diário do PSI é explicado, no mesmo dia, pelo Brent EUR e pelo EUR/USD | `gd_market_daily`, 2013–2026, dias com PSI negociado | `ret_brent_eur`, `ret_eurusd` → `ret_psi` | R², RMSE |
| **Random Forest** (preditivo: o PSI sobe amanhã?) | 68 | Classificar `label = ret_psi(t+1) > 0` | Treino até 2023, teste 2024–2026 (divisão temporal) | `psi_lag1..5`, `brent_lag1..5`, `ret_eurusd`, `psi_vs_ma50`, `psi_vs_ma200`, `brent_vs_ma50`, `vol_1y_psi`, `vol_1y_brent`, `corr_1y_psi_brent` | Accuracy, AUC, baseline "sobe sempre", importância das variáveis (top 8) |

Parâmetros do RF: `numTrees=200`, `maxDepth=5`, `seed=42`.
