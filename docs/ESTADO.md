# Estado do projeto

**Prazos** (segundo o README e o email do professor; ver [Project_Instructions.md](../Project_Instructions.md)):
- **Entrega:** o notebook em HTML no Moodle, até **16-11-2026, 23:59**.
- **Apresentação:** 15 minutos, a **17 ou 19-11-2026**, com inscrição no Moodle.

## Checklist

- [x] Fontes escolhidas e ficheiros de fallback descarregados (FRED + Yahoo, 21 ficheiros).
- [x] Notebook completo (Landing → Bronze → Silver → Gold → Análise → Conclusões), com análise mensal por empresa e inflação (07-11-2026), testado em Spark local.
- [x] Ficheiros de fallback copiados automaticamente de `dados_fallback/` para `/Volumes/workspace/oil_psi/lake/fallback/` pela secção 0 (10-11-2026; requer o repositório numa Git folder). Falta confirmar no Databricks que os 21 aparecem.
- [x] Execução local documentada para macOS e Windows ([DESENVOLVIMENTO.md](DESENVOLVIMENTO.md)); testada em macOS a 10-11-2026 (duas execuções seguidas em kernels novos, sem erros).
- [x] Notebook preparado para a apresentação (10-11-2026): índice com links, introdução com enquadramento, entrada no início de cada secção, fecho no fim das secções 1–5, 6.1 e 6.3 e uma nota "Como ler" a seguir a cada gráfico. Falta confirmar no Databricks que os links do índice funcionam.
- [x] Estilo visual NOVA IMS (13-11-2026): paleta e tipografia em [ESTILO_VISUAL.md](ESTILO_VISUAL.md), aplicadas a todos os gráficos da secção 6 e às notas "Como ler". Falta confirmar no Databricks a fonte (sem Arial, usa Liberation Sans ou DejaVu Sans).
- [x] Ingestão corrigida e mais rápida (11-11-2026): User-Agent por fonte (o FRED nunca respondia e caía sempre no fallback), pedidos às APIs em paralelo e `seed` renomeado para `fallback`. Execução local: 455 s → 46 s, 21 séries em modo `API`.
- [x] Uma só regra de janelas (`plan_windows`) para as 3 leituras e `run_pipeline` como *runner* único; introdução, 1.1 e secção 5 explicam que as 3 leituras simulam 3 dias (11-11-2026).
- [x] Avisos da execução local (11-11-2026): `pandas<3` + `pyarrow` no `requirements-local.txt` e configuração da `SparkSession` local (memória, sem barras de progresso, log `ERROR`). Os avisos passam de 436 linhas para as de arranque, com os mesmos números. Nota no notebook (secção 4) a justificar as janelas globais sem `partitionBy`. Ver [DESENVOLVIMENTO.md](DESENVOLVIMENTO.md#avisos-na-execução-local).
- [x] Boas práticas IBCS para gráficos (13-11-2026): regras, decisões do grupo (cor híbrida, datas ISO, EUR/USD) e checklist em [IBCS_GRAFICOS.md](IBCS_GRAFICOS.md).
- [x] Correções IBCS aplicadas aos 8 gráficos da secção 6 (13-11-2026; ver [IBCS_GRAFICOS.md](IBCS_GRAFICOS.md#4-avaliação-dos-gráficos-atuais-13-11-2026)), testadas em execução local completa. Falta vê-las no Databricks (fonte Liberation Sans/DejaVu Sans e o triângulo ▶ da célula 66).
- [x] Guia de tom para o texto do notebook (13-11-2026): tom, frases, pt-PT, marcas de escrita de IA a evitar e checklist com script em [TOM_ESCRITA.md](TOM_ESCRITA.md).
- [x] Secção 6 reorganizada pelas 7 perguntas, agora fechadas (13-11-2026): uma subsecção por pergunta, nota "Como ler" depois de cada gráfico e tabela, tabelas formatadas (`tabela_nova`), texto da secção 6 revisto com o [TOM_ESCRITA.md](TOM_ESCRITA.md) (*lint* sem avisos). Ver [ANALISES.md](ANALISES.md).
- [x] Pontos cegos da análise corrigidos (13-11-2026): Euro Stoxx 50 como 22.ª série e efeito C (sem a bolsa europeia) na 6.4; retornos logarítmicos de fim de mês em `gd_market_monthly`; episódios de crise por regra, com teste de ±1 mês; regressão avaliada fora da amostra (2024–2026); desfasamentos e teste dos pesos iguais com output visível. Falta confirmar no Databricks o novo fallback (`yahoo_IDX_STOXX50E.json`) e os 22 ficheiros.
- [ ] Atualizar a secção 7, [CONCLUSOES.md](CONCLUSOES.md) e "Principais resultados" do README com os números novos da secção 6 (adiado pelo grupo; ainda citam análises retiradas, como o base 100 e os meses com o Brent a variar mais de 10%).
- [x] Texto do notebook revisto com o guia de tom (13-11-2026): secção 7 reescrita com os números da execução local de 13-11-2026 (estava com os de 07-11), repetições cortadas nas células 0, 4, 9, 17, 33 e 37, travessões removidos e frases longas partidas. Corrigidos também o WTI de 20-04-2020 (−36,98 USD, o valor do FRED; o texto dizia −37,63) e o pico do PSI na 6.3 (0,64 em 2020). [CONCLUSOES.md](CONCLUSOES.md) e o README atualizados. As notas internas do grupo ficaram intactas.
- [x] Duas leituras em vez de três (13-11-2026): carga histórica até 30-06-2026 (secção 1.1) e uma incremental até hoje depois da Gold (secção 5). Texto das células 0, 1, 11, 12, 36, 37, 45 e 80 atualizado. Execução local sem erros: incremental de 2026-05-16 a 2026-11-13, 32 duplicados por série diária, Silver idêntica após reprocessamento e secção 6 igual à execução anterior. Falta correr no Databricks.
- [x] Revisão "como o professor" (13-11-2026), só texto, sem nova execução: veredicto único da hipótese no início da 7.2; Galp fecha em 0,50 na 6.5 (estava 0,51); período dos 58% e 17% de 2026 (janeiro a novembro); a Galp deixa de ser "estável" na 7.2 (cai para perto de zero em 2012–2014); a tabela de veredictos da secção 4 fica identificada como primeira leitura; o reprocessamento diz "mesmo número de linhas" (a verificação compara contagens).
- [x] Revisão do texto pelos sinais de escrita de IA das páginas da Wikipedia PT e EN (13-11-2026): listas com rótulo a negrito passadas a frases, menos negrito, notas "Como ler" em parágrafo, secção 7 mais curta, hífen do teclado em vez de − e –, sem `---` entre secções; comentários de código sem referências a `docs/` nem instruções. Só texto: nenhum número nem lógica mudou. Regras novas em [TOM_ESCRITA.md](TOM_ESCRITA.md) (secções 3, 5, 6, 7 e 9).
- [x] Notas internas da célula 0 resolvidas a pedido do grupo (13-11-2026): frase sobre a ligação entre custo/receita e a cotação; parágrafo "Um pipeline, duas leituras" simplificado.
- [x] `img/arquitetura_medallion.png` gerado de novo com "2 leituras" (13-11-2026). Saiu com outro tipo de letra (versão mais recente do Mermaid): rever o aspeto. O output guardado da célula 3 só muda na próxima execução.
- [ ] Decidir: definir ou retirar "de forma consolidada" na hipótese (célula 0); explicar numa frase a subida do Brent em 2026 e rever o nome do episódio "2026 até hoje", que acaba em 2026-03 (células 71–74 e 80).
- [ ] Decidir: mostrar na secção 6 os números que só aparecem na 7 (−1,7% e 41% dos meses, +1,7% e 65%, subperíodos da Galp, 10 das 15 empresas em 2020). Estão certos face à Gold local de 13-11-2026.
- [ ] Correr **Run all** no Databricks e ver em `ctl_ingestion_log.extract_mode` se o FRED e o Yahoo respondem (`API`) ou caem no `FALLBACK`. No Databricks fica uma pasta `lake/seed/` antiga, que se pode apagar à mão.
- [x] Revisão estática "corre no Databricks?" (13-11-2026), sem execução no Databricks. Corrigido: a célula 3 importava o `display` do IPython e substituía o do Databricks, pelo que as tabelas das secções 2 a 5 deixariam de aparecer (agora importa-o como `mostrar_imagem`). README com 22 ficheiros de fallback.
- [ ] Correr **Run all** no Databricks e corrigir eventuais erros específicos do serverless. Antes, fazer *commit* e *push* (a Git folder lê do GitHub). Pontos a ver, porque nunca correram localmente ou dependem do ambiente:
  - as tabelas do `display()` nas células 19 a 41 e o diagrama da célula 3 (3480 px de largura);
  - o código só de Delta: `MERGE INTO` (célula 24), `OPTIMIZE ... ZORDER` (célula 30), `DESCRIBE HISTORY` e `VERSION AS OF 0` (célula 44);
  - o MLlib em serverless (células 76 e 78), em especial `lr_model.summary` e `featureImportances`;
  - as tabelas `tabela_nova` (células 53, 63, 71, 76 e 78) no notebook e no HTML exportado; precisam de pandas ≥ 2.1;
  - a fonte dos gráficos sem Arial (rótulos mais largos podem sobrepor-se).
- [ ] Criar o **AI/BI Dashboard** (secção 6.2: 5 widgets sobre as tabelas `gd_*`; especificação em [ANALISES.md](ANALISES.md#62-aibi-dashboard-por-criar-no-databricks)).
- [ ] Rever as conclusões com os números da execução no Databricks ([CONCLUSOES.md](CONCLUSOES.md#3-checklist-de-revisão-após-correr-no-databricks)).
- [ ] Exportar o notebook em **HTML** (File → Export → HTML) e entregar no Moodle (até 16-11-2026, 23:59).
- [ ] Inscrever o grupo num horário de apresentação no Moodle (17 ou 19-11-2026, 15 minutos).
