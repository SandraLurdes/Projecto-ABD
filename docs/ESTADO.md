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
- [ ] Correr **Run all** no Databricks e ver em `ctl_ingestion_log.extract_mode` se o FRED e o Yahoo respondem (`API`) ou caem no `FALLBACK`. No Databricks fica uma pasta `lake/seed/` antiga, que se pode apagar à mão.
- [ ] Correr **Run all** no Databricks e corrigir eventuais erros específicos do serverless.
- [ ] Criar o **AI/BI Dashboard** (secção 6.2: 5 widgets sobre as tabelas `gd_*`; especificação em [ANALISES.md](ANALISES.md#62-aibi-dashboard-por-criar-no-databricks)).
- [ ] Rever as conclusões com os números da execução no Databricks ([CONCLUSOES.md](CONCLUSOES.md#3-checklist-de-revisão-após-correr-no-databricks)).
- [ ] Exportar o notebook em **HTML** (File → Export → HTML) e entregar no Moodle (até 16-11-2026, 23:59).
- [ ] Inscrever o grupo num horário de apresentação no Moodle (17 ou 19-11-2026, 15 minutos).
