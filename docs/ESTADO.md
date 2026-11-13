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
- [ ] Correr **Run all** no Databricks e ver em `ctl_ingestion_log.extract_mode` se o FRED e o Yahoo respondem (`API`) ou caem no `FALLBACK`. No Databricks fica uma pasta `lake/seed/` antiga, que se pode apagar à mão.
- [ ] Correr **Run all** no Databricks e corrigir eventuais erros específicos do serverless.
- [ ] Criar o **AI/BI Dashboard** (secção 6.2: 5 widgets sobre as tabelas `gd_*`; especificação em [ANALISES.md](ANALISES.md#62-aibi-dashboard-por-criar-no-databricks)).
- [ ] Rever as conclusões com os números da execução no Databricks ([CONCLUSOES.md](CONCLUSOES.md#3-checklist-de-revisão-após-correr-no-databricks)).
- [ ] Exportar o notebook em **HTML** (File → Export → HTML) e entregar no Moodle (até 16-11-2026, 23:59).
- [ ] Inscrever o grupo num horário de apresentação no Moodle (17 ou 19-11-2026, 15 minutos).
