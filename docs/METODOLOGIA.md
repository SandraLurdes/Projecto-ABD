# Metodologia: hipótese, empresas e regras de análise

São as decisões do grupo sobre **o que se testa e como**. Qualquer alteração a estas regras no notebook obriga a rever
[CONCLUSOES.md](CONCLUSOES.md).

Fonte de verdade: notebook, células 0 e 4 (hipótese, empresas), 29 e 33 (regras da Gold) e 86 (limitações).

## 1. Hipótese do grupo

> **Quando o preço do petróleo sobe, a cotação das empresas portuguesas desce — exceto a Galp.**
> Para a Galp o petróleo é receita; para as outras empresas é um custo.

**Perguntas de análise**
1. O Brent está hoje mais caro do que em 2000, em dólares, em euros e descontada a inflação?
2. A bolsa portuguesa desce quando o Brent sobe?
3. A ligação entre a bolsa portuguesa e o Brent muda ao longo do tempo?
4. O Brent penaliza as empresas portuguesas, com exceção da Galp?
5. A Galp reage ao Brent de forma diferente das restantes empresas?
6. Nas crises de 2008, da COVID-19 e da guerra na Ucrânia, a bolsa portuguesa acompanhou o Brent?
7. O Brent explica os movimentos diários do PSI e permite antecipá-los?

## 2. Empresas analisadas e sinal esperado

Cada empresa tem dois sinais esperados (catálogo `YAHOO_SYMBOLS`, célula 8):
- **Hipótese do grupo** (`oil_hypothesis`, a **testada**): `+` para a Galp e `−` para as outras. É este sinal que decide o veredicto.
- **Leitura por setor** (`sector_view`): expectativa mais fina, por grupo, usada **só para interpretar** os resultados
  (`?` = efeito ambíguo, com forças em sentidos opostos).

**A exceção: Galp**

| Empresa | `oil_hypothesis` | `sector_view` | Porquê |
|---|---|---|---|
| Galp Energia (`GALP.LS`) | **+** | + | É uma petrolífera: o petróleo mais caro aumenta as receitas. |

**As outras 14 empresas, por grupo de setor** (`oil_hypothesis` = `−` em todas)

| Grupo (`sector`) | Empresas | `sector_view` | Porquê |
|---|---|---|---|
| Consumo e retalho | Jerónimo Martins (`JMT.LS`), Sonae (`SON.LS`), Ibersol (`IBS.LS`) | − | O combustível mais caro reduz o poder de compra das famílias, que gastam menos. |
| Serviços domésticos | NOS (`NOS.LS`), CTT (`CTT.LS`) | − | Dependem da procura interna e têm custos de energia (NOS) e de transporte (CTT). |
| Eletricidade e redes | EDP (`EDP.LS`), EDP Renováveis (`EDPR.LS`), REN (`RENE.LS`) | ? | O preço da eletricidade acompanha o gás, que acompanha o petróleo, o que pode ajudar a EDP e a EDPR. A REN é um negócio regulado, pouco exposto. |
| Indústria exportadora | Navigator (`NVG.LS`), Altri (`ALTR.LS`), Semapa (`SEM.LS`, dona da Navigator e da Secil), Corticeira Amorim (`COR.LS`) | ? | Gastam muita energia e transporte (contra), mas vendem em dólares e acompanham o ciclo das matérias-primas (a favor). |
| Exposição a países petrolíferos | Mota-Engil (`EGL.LS`), BCP (`BCP.LS`) | + | Têm negócio em Angola e Moçambique (e a Mota-Engil na América Latina), economias que ganham com o petróleo caro. |

Os grupos estão ordenados do `−` mais claro para os que mais podem contrariar a hipótese (`?` e `+`).

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
- **Beta sem a bolsa europeia (efeito C):** `ret_ação ~ ret_brent_eur + ret_europa`, com o Euro Stoxx 50 (`^STOXX50E`). Retira o ciclo das
  bolsas europeias, que o mercado PT (efeito B) não separa do efeito do petróleo. Só há dados desde 2007-04, por isso o C cobre 2007–2026
  e é calculado numa agregação à parte (`n_meses_europa`).
- **Leitura dos efeitos:** `beta × 10` é a variação média da ação, em %, num mês em que o Brent (EUR) sobe 10% (é a coluna `efeito_*_10pct` das queries).
- **Períodos:** total 2000-2026 e 4 subperíodos (2000-2007, 2008-2013, 2014-2019, 2020-2026).
- **Desfasamentos:** correlação com o Brent de 1, 2 e 3 meses antes.
- **Retornos mensais de mercado:** em `gd_market_monthly`, logarítmicos de fim de mês (Brent, PSI) desde 13-11-2026, como nas empresas. O índice OCDE de
  ações PT é uma média mensal (a fonte não publica o fim do mês).
- **Episódios de crise (6.6):** o início é o fim do mês antes da janela do episódio; o fim é o mês do extremo do Brent (das ações PT na bolha
  dot-com e na troika) dentro da janela. Teste de sensibilidade: início e fim deslocados ±1 mês.
- **Qualidade:**
  - um dia com |variação| > 40% no fecho ajustado (`OUTLIER_DAILY`) marca o mês com `ret_outlier`, e esse mês fica fora das estatísticas.
    São provavelmente operações de capital mal ajustadas na fonte; apanha 2 meses da Altri, 11/2000 e 03/2005;
  - os preços ≤ 0 ficam com `dq_flag` e não são apagados (ex.: WTI a −36,98 USD a 20-04-2020).
- **ML:** divisão temporal (treino até 2023, teste de 2024 em diante), sem amostragem aleatória (não se mistura o futuro com o passado).

## 4. Limitações

- **Enviesamento de sobrevivência:** só entram as 15 empresas que estão hoje no PSI. As que saíram da bolsa (ex.: PT, BES, Cimpor, Brisa) não entram.
- O "mercado PT" é a média com pesos iguais destas empresas, não um índice ponderado.
- A CTT, a EDP Renováveis e a REN têm um histórico mais curto (155 a 232 meses).
- Não há PSI diário antes de 2013. Para 2000–2026 usa-se o índice mensal da OCDE (ver [FONTES.md](FONTES.md)).
