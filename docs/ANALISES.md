# Catálogo de análises (secção 6 do notebook)

Este documento indica que análises existem, a que pergunta responde cada uma e que tabelas `gd_*` lê.
- As colunas das tabelas estão em [TABELAS.md](TABELAS.md).
- Os resultados estão em [CONCLUSOES.md](CONCLUSOES.md).

A secção 6 está organizada **por pergunta de análise** (P1–P7, listadas em [METODOLOGIA.md](METODOLOGIA.md#1-hipótese-do-grupo)):
uma subsecção 6.N por pergunta. Cada subsecção tem:
- uma célula com a pergunta;
- no máximo dois artefactos (tabela ou gráfico);
- uma célula **"Resposta à pergunta N"**, com as notas "Como ler" e a resposta.

Regras desta secção:
- Os gráficos são matplotlib, porque os do `display()` dependem de configuração na interface e não ficam no HTML exportado.
- As tabelas são `%sql` ou PySpark.
- Nenhuma subsecção passa de dois artefactos: uma análise nova tem de substituir outra ou responder a uma pergunta nova.

## 6.0 Preparação (célula 47)

A célula de preparação define, para toda a secção:
- os imports;
- a paleta NOVA IMS (ver [ESTILO_VISUAL.md](ESTILO_VISUAL.md)): `HIERARQUIA` (verde, azul, rosa, preto, por ordem de importância), `C_GALP` (laranja, sempre), `cores_por_ordem(...)` e `SECTOR_ORDER`;
- os episódios sombreados (`CRISES`) e a função `sombrear_crises(ax)`;
- os ajudantes IBCS `rotular_fim(ax, itens)` (nome no fim das linhas) e `finalizar(fig, fonte)` (rodapé com a fonte), ver [IBCS_GRAFICOS.md](IBCS_GRAFICOS.md);
- o estilo (`rcParams`);
- os DataFrames pandas partilhados: `pm` (`gd_market_monthly`), `pdl` (`gd_market_daily` desde 2013) e `eqm` (`gd_equity_monthly`);
- `base_mes`, o último mês com IHPC publicado.

## Análises por pergunta

| Sub | Pergunta | Célula | Artefacto | Tabelas |
|---|---|---|---|---|
| 6.1 | P1 Preço do Brent desde 2000 | 49 | Dois painéis de médias anuais. Esquerda: câmbio, com o Brent USD e o Brent EUR (eixo da esquerda, a partir de 0) e a taxa EUR/USD num segundo eixo à direita, alinhado em dois pontos (taxa de 2000 ↔ preço de 2000; taxa máxima ↔ preço máximo), por isso sem começar em 0 (gráfico de dois eixos pedido pelo grupo, para comparar a forma). Direita: inflação, com o Brent em EUR de cada ano vs em EUR de 2000 (calculado na célula com o `hicp` mensal), a diferença sombreada e os valores de 2000/2008/2012/2016/2022/2026 | `gd_market_yearly`, `gd_market_monthly` |
| 6.2 | P2 Bolsa portuguesa e Brent | 52 | Uma figura: à esquerda, a dispersão dos retornos mensais (logarítmicos, apresentados como variação simples) Brent EUR × ações PT (OCDE), sem reta (a relação é fraca), com r, r² e o *n* no título; à direita, *small multiples* por setor (média pesos iguais do `ret_m_stat` das empresas do setor × `ret_brent_eur_m` de `gd_equity_monthly`; sem o cabaz), com r e r² de cada setor (um setor com uma só empresa leva o nome dela: Galp Energia). Escala comum de ±40%; os meses fora da escala ficam na margem, com círculo vazio. Pontos no verde da NOVA IMS (cor principal); o painel da Galp a laranja (`C_GALP`) | `gd_market_monthly`, `gd_equity_monthly` |
| | | 53 (PySpark + tabela formatada) | Tabela de correlações: diária PSI vs Brent EUR e vs EUR/USD (2013–2026); mensal OCDE vs Brent (2000–2026); mensal PSI vs Brent; níveis mensais OCDE vs Brent | `gd_market_daily`, `gd_market_monthly` |
| 6.3 | P3 Ligação ao longo do tempo | 56 | Linhas: correlação móvel PSI–Brent de 1 ano (diária, 2014+) e de 36 meses do cabaz sem Galp (mensal), com as crises sombreadas. Por baixo, uma linha impressa com o teste dos pesos iguais: correlação mensal com o Brent do cabaz e do PSI (fim de mês, 2013-05+) e entre os dois | `gd_market_daily`, `gd_equity_monthly` |
| | | 58 | Mapa de calor: correlação móvel de 36 meses de cada empresa com o Brent, no fim de cada ano, agrupada por setor | `gd_equity_monthly` |
| 6.4 | P4 Efeito por empresa | 61 | Barras em 3 painéis com a mesma escala: A efeito total, B sem a bolsa portuguesa, C sem a bolsa europeia (Euro Stoxx 50, 2007–2026), por empresa (+10% no Brent), com ±2 erros-padrão; Galp a laranja, cabaz a verde e as restantes a cinzento, agrupadas por setor | `gd_oil_sensitivity` |
| | | 63 (`spark.sql` + tabela formatada) | Tabela formatada (`tabela_nova`) e curta, pela ordem do gráfico: sinal esperado, efeitos A (total), B (sem a bolsa portuguesa) e C (sem a bolsa europeia) para +10% no Brent e o veredicto de cada um (Confirma a verde claro, Contraria a negrito, Sem efeito a cinzento; total 2000-2026) | `gd_oil_sensitivity` |
| 6.5 | P5 Galp e restantes empresas | 66 | Linhas: correlação móvel de 36 meses da Galp vs o cabaz sem Galp, com as crises sombreadas | `gd_equity_monthly` |
| | | 68 | Colunas agrupadas: variação média mensal da Galp e do cabaz sem Galp em 4 grupos de meses, consoante o Brent EUR cai > 10%, cai 0–10%, sobe 0–10% ou sobe > 10% (meses comuns aos dois, desde 2006-11; `ret_m_stat` e `ret_brent_eur_m` passados a variação simples), com o nº de meses de cada grupo e rótulos de dados | `gd_equity_monthly` |
| 6.6 | P6 Bolsa e Brent nas crises | 71 (`spark.sql` + tabela formatada) | Tabela: variação do Brent EUR, das ações PT (OCDE) e do PSI em 9 episódios (dot-com, petróleo 2007-08, crise de 2008, troika, colapso de 2014-16, COVID-19, recuperação, Ucrânia, 2026), com a duração em meses. Datas por regra: início = fim do mês antes da janela; fim = mês do extremo do Brent (das ações PT na dot-com e na troika) dentro da janela; valores de fim de mês (OCDE: média mensal, com o último valor publicado nos meses recentes). Por cima, uma linha com o teste de sensibilidade (início e fim ±1 mês) | `gd_market_monthly` |
| | | 73 | Barras agrupadas horizontais com rótulos: Brent EUR vs ações PT por episódio, com marcador de outlier acima de +120% (usa o `ep_pd` da célula 71) | `gd_market_monthly` |
| 6.7 | P7 Explicar e antecipar o PSI | 76, 78 | Modelos de ML (ver abaixo) | `gd_market_daily` |

A query das crises está numa célula Python (`spark.sql`), e não em `%sql`, para o gráfico da célula 73 reutilizar o mesmo DataFrame
sem repetir a lista de episódios.

## AI/BI Dashboard (por criar no Databricks)

Cria-se em *New → Dashboard*, com as fontes de dados `gd_market_daily`, `gd_market_monthly`, `gd_market_yearly`, `gd_equity_monthly` e
`gd_oil_sensitivity`. Os widgets propostos estão na introdução da secção 6 do notebook (célula 46):

| # | Widget | Pergunta | Tipo | Tabela | Campos sugeridos |
|---|---|---|---|---|---|
| W1 | Brent mensal 2000–2026 em USD, em EUR e em EUR a preços de hoje | P1 | linhas | `gd_market_monthly` | `month_date`; `brent_usd_avg`, `brent_eur_avg`, `brent_eur_real` |
| W2 | Correlação móvel de 1 ano PSI–Brent | P3 | linha | `gd_market_daily` | `date`; `corr_1y_psi_brent` |
| W3 | Efeito do petróleo por empresa, descontando a bolsa | P4 | barras coloridas por setor | `gd_oil_sensitivity` | `name`; `beta_sem_mercado × 10`; cor = `sector`; filtro `periodo = 'Total 2000-2026'` |
| W4 | Correlação móvel de 36 meses com o Brent, com filtro por empresa | P3, P5 | linhas | `gd_equity_monthly` | `month_date`; `corr_36m_brent`; filtro = `name` |
| W5 | Tabela de resumo anual | P1, P6 | tabela | `gd_market_yearly` | `year`, médias do Brent (USD, EUR, EUR real), variações anuais do Brent EUR, do PSI e do índice OCDE |

A coluna "Campos sugeridos" mapeia cada widget para colunas existentes. O notebook só descreve o widget.

## 6.7 Modelos de Machine Learning (Spark MLlib)

A pergunta 7 é sobretudo explicativa:
- a **regressão linear** é a resposta principal, treinada até 2023 e avaliada também em 2024–2026 (R² fora da amostra);
- o **Random Forest** é secundário e mostra que o petróleo passado não ajuda a prever o dia seguinte.

| Modelo | Célula | Objetivo | Dados | Variáveis | Avaliação |
|---|---|---|---|---|---|
| **Regressão linear** (explicativo) | 76 (tabela formatada: R² de treino e de teste, efeitos de +10%, dias) | Quanto do retorno diário do PSI é explicado, no mesmo dia, pelo Brent EUR e pelo EUR/USD | `gd_market_daily`, dias com PSI negociado; treino até 2023, teste 2024–2026 | `ret_brent_eur`, `ret_eurusd` → `ret_psi` | R² de treino e de teste (`RegressionEvaluator`) |
| **Random Forest** (preditivo: o PSI sobe amanhã?) | 78 (tabela formatada: accuracy e AUC vs referência; em texto, as 5 variáveis mais importantes e o intervalo das correlações desfasadas `corr_lag1..3` de `gd_oil_sensitivity`) | Classificar `label = ret_psi(t+1) > 0` | Treino até 2023, teste 2024–2026 (divisão temporal) | `psi_lag1..5`, `brent_lag1..5`, `ret_eurusd`, `psi_vs_ma50`, `psi_vs_ma200`, `brent_vs_ma50`, `vol_1y_psi`, `vol_1y_brent`, `corr_1y_psi_brent` | Accuracy, AUC, baseline "sobe sempre", importância das variáveis (top 8) |

Parâmetros do RF: `numTrees=200`, `maxDepth=5`, `seed=42`.

## Notas de leitura

Cada gráfico e cada tabela da secção 6 tem logo a seguir uma nota *Como ler* (texto segundo [TOM_ESCRITA.md](TOM_ESCRITA.md)) com três partes: **o gráfico** (o que mostra), **as linhas / barras / pontos / cores** (o que é cada elemento) e **conclusões** (com os números do output). A última nota de cada subsecção chama-se "Resposta à pergunta N" e termina com a **Resposta:**, que começa por "Sim.", "Não." ou "Em parte.". As perguntas são fechadas por isso. As tabelas de resultados usam `tabela_nova` (ver [ESTILO_VISUAL.md](ESTILO_VISUAL.md#8-tabelas)).
