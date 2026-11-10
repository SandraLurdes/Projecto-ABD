# Estado do projeto

**Prazos** (segundo o README e o email do professor; ver [Project_Instructions.md](../Project_Instructions.md)):
- **Entrega:** o notebook em HTML no Moodle, até **16-11-2026, 23:59**.
- **Apresentação:** 15 minutos, a **17 ou 19-11-2026**, com inscrição no Moodle.

## Checklist

- [x] Fontes escolhidas e seeds descarregados (FRED + Yahoo, 21 ficheiros).
- [x] Notebook completo (Landing → Bronze → Silver → Gold → Análise → Conclusões), com análise mensal por empresa e inflação (07-11-2026), testado em Spark local.
- [x] Seeds copiados automaticamente de `dados_fonte/` para `/Volumes/workspace/oil_psi/lake/seed/` pela secção 0 (10-11-2026; requer o repositório numa Git folder). Falta confirmar no Databricks que os 21 aparecem.
- [x] Execução local documentada para macOS e Windows ([DESENVOLVIMENTO.md](DESENVOLVIMENTO.md)); testada em macOS a 10-11-2026 (duas execuções seguidas em kernels novos, sem erros).
- [x] Notebook preparado para a apresentação (10-11-2026): índice com links, introdução com enquadramento, entrada no início de cada secção, fecho no fim das secções 1–5, 6.1 e 6.3 e uma nota "Como ler" a seguir a cada gráfico. Falta confirmar no Databricks que os links do índice funcionam.
- [ ] Correr **Run all** no Databricks e corrigir eventuais erros específicos do serverless.
- [ ] Criar o **AI/BI Dashboard** (secção 6.2: 5 widgets sobre as tabelas `gd_*`; especificação em [ANALISES.md](ANALISES.md#62-aibi-dashboard-por-criar-no-databricks)).
- [ ] Rever as conclusões com os números da execução no Databricks ([CONCLUSOES.md](CONCLUSOES.md#3-checklist-de-revisão-após-correr-no-databricks)).
- [ ] Exportar o notebook em **HTML** (File → Export → HTML) e entregar no Moodle (até 16-11-2026, 23:59).
- [ ] Inscrever o grupo num horário de apresentação no Moodle (17 ou 19-11-2026, 15 minutos).
