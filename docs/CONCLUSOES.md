# Resultados e conclusões

**Proveniência:** execução local de 13-11-2026, 16:34 (PySpark 4.3 + Java 17, tabelas em Parquet). As 22 séries vieram das APIs (modo `API`)
nas 2 leituras, com dados até 13-11-2026. Os números foram lidos das saídas guardadas no notebook e das tabelas Gold locais
(`lakehouse_oil_psi/warehouse/oil_psi.db/gd_*`).

**A fonte de verdade é o notebook, secção 7.** Este documento resume os números e indica onde estão no texto.
**Se os dados mudarem** (por exemplo, depois do Run all no Databricks), é preciso rever os números citados nas conclusões do notebook,
aqui e no resumo do [README](../README.md).

## 1. Números atuais

- **Galp:** corr. mensal 0,42; +2,7% por cada +10% no Brent EUR (t = 7,1); sem a bolsa PT +2,1% (t = 5,7); sem a bolsa europeia +2,2%
  (t = 5,9). Estável desde 2008 (subperíodos 0,43–0,52). **Confirma a hipótese.**
- **As outras 14 empresas:** todas com correlação positiva (0,02–0,27); 8 com efeito positivo e significativo, nenhuma negativa significativa.
  Cabaz sem a Galp: +1,0% por +10% (t = 4,2). A hipótese **não se confirma** no efeito total (ciclo económico comum).
- **Sem a bolsa PT (efeito B):** 7/14 negativas (EDP, EDPR, REN, NOS, CTT, Jerónimo Martins, Sonae); significativas a REN (−0,4%, t = −2,5)
  e os CTT (−1,1%, t = −2,2). Semapa e Ibersol positivas e significativas.
- **Sem a bolsa europeia (efeito C, Euro Stoxx 50, 2007–2026):** só a Galp confirma; Corticeira Amorim, Navigator, Semapa, Ibersol e o
  cabaz contrariam; nenhuma empresa desce de forma significativa.
- **Ao longo do tempo:** correlação de 36 meses do cabaz sem Galp em 0,73 no fim de 2020 (crise de procura) e −0,34 em 11-2026 (subida
  do Brent); a Galp em 0,50. Correlação móvel de 1 ano do PSI: picos de 0,56 (2016) e 0,64 (2020), −0,13 em 11-2026.
- **Desfasamentos:** correlações com o Brent de 1–3 meses antes entre −0,12 e 0,09 (nulas).
- **Inflação:** Brent em euros de 2000: 31 € (2000), 51 € (2008), 64 € (2012), 62 € (2022), 47 € (2026).
- **Bolsa vs Brent EUR:** corr. mensal 0,31 (ações PT da OCDE, 2000–2026), 0,32 (PSI mensal, 2013–2026), diária 0,21 (PSI); níveis 0,51.
  Em 2026 (até 13-11) o Brent em EUR subiu 58% e o PSI 17%.
- **ML:** regressão linear com R² de 7,3% no treino (2013–2023; +10% no Brent ⇒ +1,03% no PSI no mesmo dia) e −15,4% no teste
  (2024–2026). Random Forest (o PSI sobe amanhã?): accuracy 47,5% e AUC 0,44, abaixo da regra "sobe sempre" (53,6%).

## 2. Mapa das conclusões do notebook (secção 7)

A secção 7 abre com uma tabela que resume a resposta a cada pergunta (Sim./Não./Em parte., como na secção 6).

### 7.1 Sobre os dados
| # | Conclusão | Números citados |
|---|---|---|
| 1 | Petróleo: de 9 a 144 USD em 26 anos | Brent 28,6 USD (2000), 144 (07-2008), 34 (fim de 2008), 26 (2016), 9 (04-2020), 101 (2022), 97 e máx. 138 (2026); em euros de 2000: 47 € (2026), 31 € (2000), 64 € (2012), 62 € (2022), 51 € (2008) |
| 2 | Em euros, o choque de 2008 foi menor | 2008: 97 USD, mas 65 € (1,47 USD por EUR); EUR/USD vs PSI diário 0,04 |
| 3 | A relação é positiva, fraca e instável | 0,31 mensal, 0,21 diária, níveis 0,51; picos de 0,56 (2016) e 0,64 (2020); 2026: Brent EUR +58%, PSI +17% |
| 4 | O petróleo acompanha o ciclo económico | Brent < −10%: ações −1,7% (sobem em 41% dos meses); Brent > +10%: +1,7% (65%); 2008 −69% / −43%; COVID −78% / −14%; pós-COVID +334% / +46% |
| 5 | Há exceções nas subidas do Brent e nas crises internas | 2007-08: Brent +97%, ações −4% (sinal sensível às datas); troika: +37% / −33% |
| 6 | O petróleo explica pouco do PSI e não o antecipa | R² 7,3% (treino), −15,4% (teste), +1,03% por +10%; RF accuracy 47,5%, AUC 0,44, regra "sobe sempre" 53,6% |

Os números da conclusão 4 (meses com o Brent a variar mais de 10%) não aparecem na secção 6. Foram calculados sobre `gd_market_monthly`
(retornos do índice OCDE e do Brent EUR, convertidos em variação simples), com 39 e 52 meses.

### 7.2 Sobre a hipótese do grupo
| # | Conclusão | Números citados |
|---|---|---|
| 7 | A Galp confirma | corr. 0,42; +2,7% (t 7,1); sem bolsa PT +2,1% (t 5,7); sem bolsa europeia +2,2% (t 5,9); subperíodos 0,43–0,52; Brent > +10%: Galp +7,0%, cabaz +2,0% |
| 8 | As outras 14 não confirmam no efeito total | corr. 0,02–0,27; 8 *Contraria*; cabaz +1,0% (t 4,2) |
| 9 | Sem a bolsa PT, o efeito "custo" aparece em poucas empresas | 7/14 negativas; REN −0,4% (t −2,5), CTT −1,1% (t −2,2); Semapa e Ibersol *Contraria*; sem bolsa europeia, nenhuma negativa significativa |
| 10 | A relação depende do tipo de choque | cabaz 0,73 (fim de 2020), 10/15 empresas acima de 0,5; fim de 2025: EDP −0,56, EDPR −0,50, JMT −0,41; 11-2026: empresas entre −0,31 e 0,03, cabaz −0,34, Galp 0,50 |
| 11 | O efeito não demora a aparecer | lags −0,12 a 0,09 |
| — | Limitações | sobrevivência; mercado PT com pesos iguais (correlação de 0,90 com o PSI); CTT/EDPR/REN com 155–232 meses |

### 7.3 Sobre o processo
- Medallion. 2 leituras de 22 séries (5 FRED, 17 Yahoo), a carga histórica e uma incremental, com 45 dias de sobreposição: 32 duplicados por série diária, resolvidos pelo `MERGE`.
- Silver reconstruída a partir da Bronze com o mesmo número de linhas em cada tabela (a verificação compara contagens).
- Qualidade: dias sem cotação removidos (500 no Brent), tipos convertidos, WTI −36,98 USD com `dq_flag`, fecho ajustado, 2 meses da Altri
  excluídos, linhagem e `ctl_ingestion_log`.
- Não há PSI diário antes de 2013. A solução foi combinar o índice OCDE (2000–2026) com o PSI diário (2013–2026).

### 7.4 Sobre a tecnologia
- **Spark:** CSV e JSON aninhado (`explode`/`posexplode`), janelas, MLlib e regressões só com agregações. Volume: ~7 mil dias e
  ~94 mil cotações diárias das 15 empresas.
- **Delta Lake:** ACID, `MERGE`, time travel, `OPTIMIZE`/`ZORDER`.
- **Databricks Free Edition:** internet restrita (daí o fallback), o serverless não permite `cache()`, a escala de compute é reduzida.
- **Próximos passos:**
  - Job diário;
  - Auto Loader ou Lakeflow Declarative Pipelines;
  - taxas de juro, gás natural e as empresas que saíram da bolsa;
  - modelar a volatilidade em vez da direção do mercado.

## 3. Checklist de revisão após correr no Databricks

1. Comparar a tabela da 6.4 (`gd_oil_sensitivity`, total) com a secção 1 deste documento.
2. Rever as conclusões 1–11 do notebook e os números da tabela acima, e também os números citados nos fechos de secção e nas notas
   "Como ler" dos gráficos.
3. Atualizar este ficheiro, o resumo do README e a proveniência no topo.
