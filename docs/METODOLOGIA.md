# Metodologia: hipótese, empresas e regras de análise

São as decisões do grupo sobre **o que se testa e como**. Qualquer alteração a estas regras no notebook obriga a rever
[CONCLUSOES.md](CONCLUSOES.md).

Fonte de verdade: notebook, células 0 e 2 (hipótese, empresas), 24 e 27 (regras da Gold) e 69 (limitações).

## 1. Hipótese do grupo

> **Quando o preço do petróleo sobe, a cotação das empresas portuguesas desce — exceto a Galp.**
> Para a Galp o petróleo é receita; para as outras empresas é um custo.

**Perguntas de análise**
1. Como evoluiu o preço do petróleo desde 2000, em dólares, em euros e em euros a preços de hoje (descontando a inflação)?
2. O mercado acionista português acompanha o petróleo? Em que períodos a relação é mais forte?
3. Como reage cada empresa ao petróleo, mês a mês? A Galp comporta-se de forma diferente das restantes? E depois de descontar o movimento geral da bolsa?
4. Como se comportaram os mercados nas grandes crises (2008, 2014-16, COVID-19, guerra na Ucrânia)?
5. Os movimentos do petróleo ajudam a explicar ou a prever os movimentos do PSI?

## 2. Empresas analisadas e sinal esperado

| Empresa | Símbolo | Grupo (setor) | Hipótese do grupo (`oil_hypothesis`) | Leitura por setor (`sector_view`) | Porquê |
|---|---|---|---|---|---|
| Galp Energia | `GALP.LS` | Petróleo e gás | **+** | + | O petróleo é receita |
| EDP | `EDP.LS` | Eletricidade e redes | − | ? | O preço da eletricidade acompanha o gás, que acompanha o petróleo |
| EDP Renováveis | `EDPR.LS` | Eletricidade e redes | − | ? | Idem |
| REN | `RENE.LS` | Eletricidade e redes | − | ? | Negócio regulado, pouco exposto |
| Navigator | `NVG.LS` | Indústria exportadora | − | ? | Gasta muita energia, mas vende em dólares e acompanha o ciclo das matérias-primas |
| Altri | `ALTR.LS` | Indústria exportadora | − | ? | Idem |
| Semapa | `SEM.LS` | Indústria exportadora | − | ? | Idem (dona da Navigator e da Secil) |
| Corticeira Amorim | `COR.LS` | Indústria exportadora | − | ? | Exportadora, custos de transporte |
| Jerónimo Martins | `JMT.LS` | Consumo e retalho | − | − | Combustível mais caro reduz o poder de compra |
| Sonae | `SON.LS` | Consumo e retalho | − | − | Idem |
| Ibersol | `IBS.LS` | Consumo e retalho | − | − | Idem |
| NOS | `NOS.LS` | Serviços domésticos | − | − | Custos de energia, procura interna |
| CTT | `CTT.LS` | Serviços domésticos | − | − | Custos de transporte |
| Mota-Engil | `EGL.LS` | Exposição a países petrolíferos | − | + | Negócio em Angola, Moçambique e América Latina, que ganham com o petróleo caro |
| BCP | `BCP.LS` | Exposição a países petrolíferos | − | + | Participação em Angola e Moçambique |

- A hipótese **testada** é a do grupo: `+` para a Galp e `−` para as outras. A `sector_view` é uma expectativa por setor,
  usada **só para interpretar** os resultados.
- Datas de entrada em bolsa (início da série): Galp 2006, EDPR 2008, REN 2007, CTT 2013, Mota-Engil 2001; as outras estão desde 2000
  (ver [FONTES.md](FONTES.md)).
- O cabaz `PT_EX_GALP` (média das 14 empresas sem a Galp) é tratado como mais um instrumento, com sinal esperado `−`.

## 3. Regras metodológicas

- **A análise por empresa é mensal** (decisão de 07-11-2026): retornos logarítmicos com o fecho ajustado do último dia de negociação
  do mês. O dia a dia tem demasiado ruído.
- **As correlações, os betas e os testes fazem-se sempre sobre variações** (retornos logarítmicos), **nunca sobre níveis de preço**,
  por causa das correlações espúrias: duas séries que sobem durante 20 anos parecem correlacionadas mesmo sem relação entre elas.
- **Fecho ajustado:** nas empresas usa-se o `adjclose`. Em períodos longos, o fecho simples ignora os dividendos e as operações de capital
  (ex.: aumentos de capital do BCP) e distorce os retornos.
- O petróleo compara-se com o PSI e com as empresas **em EUR**: `brent_eur = brent_usd / eurusd`.
- **Inflação:** o IHPC de Portugal só serve para os gráficos de evolução em euros de hoje (`brent_eur_real`, `adj_close_real`). As
  correlações usam retornos nominais, porque a inflação mensal (~0,2%) afeta por igual as duas séries e quase não muda os resultados.
  A paridade de poder de compra não se aplica (um só país, uma só moeda).
- **Significância:** |t| ≥ 2.
- **Veredicto** (`_verdict`):
  - *Confirma*: o sinal é o esperado (− para as empresas, + para a Galp) e é significativo;
  - *Contraria*: é significativo, mas com o sinal oposto;
  - *Sem efeito significativo*: todos os outros casos;
  - *Dados insuficientes*: menos de 24 meses (`MIN_MONTHS`).
- **Beta (efeito total):** `ret_ação ~ ret_brent_eur`.
- **Beta sem mercado:** `ret_ação ~ ret_brent_eur + ret_mercado_PT`. O mercado PT é a média com pesos iguais das **outras** empresas nesse mês
  (sem a própria). Isola o efeito "custo do petróleo" do efeito "ciclo económico", que faz subir e descer o petróleo e as ações ao mesmo tempo.
- **Leitura dos efeitos:** `beta × 10` é a variação média da ação, em %, num mês em que o Brent (EUR) sobe 10% (é a coluna `efeito_*_10pct` das queries).
- **Períodos:** total 2000-2026 e 4 subperíodos (2000-2007, 2008-2013, 2014-2019, 2020-2026).
- **Desfasamentos:** correlação com o Brent de 1, 2 e 3 meses antes.
- **Qualidade:**
  - um dia com |variação| > 40% no fecho ajustado (`OUTLIER_DAILY`) marca o mês com `ret_outlier`, e esse mês fica fora das estatísticas.
    São provavelmente operações de capital mal ajustadas na fonte; apanha 2 meses da Altri, 11/2000 e 03/2005;
  - os preços ≤ 0 ficam com `dq_flag` e não são apagados (ex.: WTI a −37,63 USD a 20-04-2020).
- **ML:** divisão temporal (treino até 2023, teste de 2024 em diante), sem amostragem aleatória (não se mistura o futuro com o passado).

## 4. Limitações

- **Enviesamento de sobrevivência:** só entram as 15 empresas que estão hoje no PSI. As que saíram da bolsa (ex.: PT, BES, Cimpor, Brisa) não entram.
- O "mercado PT" é a média com pesos iguais destas empresas, não um índice ponderado.
- A CTT, a EDP Renováveis e a REN têm um histórico mais curto (155 a 232 meses).
- Não há PSI diário antes de 2013. Para 2000–2026 usa-se o índice mensal da OCDE (ver [FONTES.md](FONTES.md)).
