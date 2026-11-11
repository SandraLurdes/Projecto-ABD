# Resultados e conclusões

**Proveniência:** execução local de 07-11-2026 com os ficheiros de fallback e retornos mensais (PySpark 4.3 + Java 17, modo forçado, então chamado SEED, tabelas
em memória: o Windows sem `winutils.exe` não grava tabelas). As regressões do Spark foram conferidas com numpy (diferença < 1e-14).

**A fonte de verdade é o notebook, secção 7.** Este documento resume os números e indica onde estão no texto.
**Se os dados mudarem** (por exemplo, se a API funcionar no Databricks), é preciso rever os números citados nas conclusões do notebook,
aqui e no resumo do [README](../README.md).

## 1. Números atuais

- **Galp:** corr. mensal 0,42; +2,7% por cada +10% no Brent EUR (beta 0,27, t = 7,1); sem mercado +2,1% (t = 5,6). Estável em todos os
  subperíodos (0,43–0,52). **Confirma a hipótese.**
- **As outras 14 empresas:** todas com correlação positiva (0,01–0,27); 8 com beta positivo e significativo, nenhuma negativa significativa.
  Cabaz sem a Galp: +1,0% por +10% (t = 4,3). A hipótese **não se confirma** no efeito total (ciclo económico comum).
- **Sem o efeito do mercado:** 7/14 negativas (EDP, EDPR, REN, NOS, CTT, Jerónimo Martins, Sonae); significativas REN (−0,4%, t = −2,4) e
  CTT (−1,1%, t = −2,4). Exportadoras e BCP/Mota-Engil continuam positivas; Semapa e Ibersol positivas e significativas.
- **Ao longo do tempo:** correlação de 36 meses do cabaz sem Galp em 0,73 no fim de 2020 (crise de procura) e −0,32 em 11/2026 (choque de
  oferta, Brent EUR +83% em 2026); a Galp ficou em +0,50.
- **Desfasamentos:** correlações com o Brent de 1–3 meses antes entre −0,12 e 0,09 (nulas).
- **Inflação:** Brent em euros de hoje — 2008: 92 €; 2012: 115 €; 2022: 111 €; 2000: 56 €.
- **PSI vs Brent EUR:** corr. diária 0,21 e mensal 0,25. Em 2026 o Brent em EUR subiu 83% e o PSI 15%.
  `[FLAG]` Na Q5 e na conclusão 3 do notebook, o 0,25 é "mensal 2000–2026" e corresponde às **ações PT (OCDE)** vs Brent EUR. O PSI só
  existe desde 2013 e tem uma linha própria na Q5 ("Mensal 2013–2026: PSI vs Brent EUR"). Confirmar qual dos dois é o 0,25 na próxima execução.
- **ML:** a regressão linear tem R² = 5% (+10% no Brent ⇒ +0,8% no PSI no mesmo dia). O Random Forest (o PSI sobe amanhã?) tem accuracy
  47,5% e AUC 0,44, abaixo da baseline de 53,4% ("sobe sempre").

## 2. Mapa das conclusões do notebook (secção 7)

### 7.1 Sobre os dados
| # | Conclusão | Números citados |
|---|---|---|
| 1 | Petróleo: 26 anos de ciclos muito violentos | Brent 28,6 USD (2000), 144 (07/2008), 34 (fim de 2008), 26 (2016), 9 (04/2020), 101 (2022), 97 e máx. 138 (2026); em euros de hoje 92 € (2008), 115 € (2012), 111 € (2022), 56 € (2000) |
| 2 | O câmbio importa | 2008: 97 USD, mas 65 EUR; EUR/USD vs PSI diário 0,04 |
| 3 | A relação existe, mas é fraca e instável | 0,21 diária, 0,25 mensal, níveis 0,51, pico de 0,55 em 2016, negativa em 2024–26; 2026: Brent EUR +83%, PSI +15% |
| 4 | O petróleo é um termómetro do ciclo económico | Brent < −10%: ações −1,6% (sobem em 42% dos meses); Brent > +10%: +2,4% (72%); 2008 −60% / −51%; COVID −70% / −17% |
| 5 | Há exceções nos choques de oferta ou internos | 2007-08: Brent +104%, ações −20%; troika: +42% / −34% |
| 6 | Prever o PSI com o petróleo não funciona | R² 5%, +0,8% por +10%; RF accuracy 47,5%, AUC 0,44, baseline 53,4% |

### 7.2 Sobre a hipótese do grupo
| # | Conclusão | Números citados |
|---|---|---|
| 7 | A Galp confirma | corr. 0,42; +2,7% (t 7,1); sem mercado +2,1% (t 5,6); subperíodos 0,43–0,52; desde 11/2006, a preços reais: Galp ×3,9, cabaz ×2,0, Brent ×1,3 |
| 8 | As outras 14 não confirmam no efeito total | corr. 0,01–0,27; 8 *Contraria*; cabaz +1,0% (t 4,3) |
| 9 | Sem a bolsa, aparece o efeito "custo" em alguns setores | 7/14 negativas; REN −0,4% (t −2,4), CTT −1,1% (t −2,4); Semapa e Ibersol *Contraria*; efeito < 1,2% |
| 10 | A relação depende do tipo de choque | cabaz 0,73 (fim de 2020); fim de 2025: EDP −0,56, EDPR −0,50, JMT −0,41; 2026: cabaz −0,32, Galp 0,50 |
| 11 | O efeito não demora a aparecer | lags −0,12 a 0,09 |
| — | Limitações | sobrevivência; mercado PT com pesos iguais; CTT/EDPR/REN com 155–232 meses |

### 7.3 Sobre o processo
- Medallion de ponta a ponta. 3 leituras de 21 séries com 45 dias de sobreposição: 65 duplicados por série diária, resolvidos pelo `MERGE`.
- Silver reconstruída a partir da Bronze com resultado idêntico.
- Qualidade: feriados removidos, tipos convertidos, WTI −37,63 com `dq_flag`, fecho ajustado, 2 meses da Altri excluídos, linhagem e `ctl_ingestion_log`.
- Não há PSI diário antes de 2013. A solução foi combinar o índice OCDE (2000–2026) com o PSI diário (2013–2026).

### 7.4 Sobre a tecnologia
- **Spark:** CSV e JSON aninhado (`explode`/`posexplode`), janelas, MLlib e regressões só com agregações. Volume: ~7 mil dias e
  ~97 mil cotações diárias de empresas; o mesmo código escala.
- **Delta Lake:** ACID, `MERGE`, time travel, `OPTIMIZE`/`ZORDER`.
- **Databricks Free Edition:** internet restrita (daí o fallback), o serverless não permite `cache()`, a escala de compute é reduzida.
- **Melhorias futuras:**
  - Job diário;
  - Auto Loader / Lakeflow Declarative Pipelines;
  - mais variáveis (taxas de juro, gás natural) e as empresas que saíram da bolsa;
  - modelar a volatilidade em vez da direção do mercado.

## 3. Checklist de revisão após correr no Databricks

1. Comparar a tabela da Q6 (`gd_oil_sensitivity`, total) com a secção 1 deste documento.
2. Rever as conclusões 1–11 do notebook e os números da tabela acima, e também os números citados nos fechos de secção (5, 6.1, 6.3) e nas notas "Como ler" dos gráficos.
3. Atualizar este ficheiro, o resumo do README e a proveniência no topo.
