# Boas práticas IBCS para gráficos

Regras de **notação e escolha de gráficos** do projeto, segundo as *International Business Communication Standards* (IBCS v2.0).
O [ESTILO_VISUAL.md](ESTILO_VISUAL.md) diz **com que cores e fontes** se desenha. Este documento diz **o que se desenha e como se lê**:
tipo de gráfico, títulos, eixos, rótulos, escalas e realces.
**Todo o gráfico, novo ou alterado, segue e revê este documento** (ver a [regra obrigatória](#7-regra-obrigatória) no fim).

## 1. Origem e âmbito

- Fonte: *IBCS Version 2.0* (IBCS Association, 2026-07-16; CC BY-SA 4.0), alinhada com a norma **ISO 24896** "Notation for business
  reporting". O PDF está na raiz do repositório (`IBCS_v2_final_2026-07-16_*.pdf`), mas **fora do git**: é uma cópia licenciada a um
  elemento do grupo. A versão pública está em <https://www.ibcs.com/standards>.
- Cada regra abaixo cita o ID IBCS (ex.: **CH 2.1**), para se poder confirmar no PDF. O que é interpretação nossa, para o contexto de
  um notebook matplotlib, está marcado com *(nosso)*.
- As IBCS organizam-se na fórmula **SUCCESS**:
  - notação: **S**IMPLIFY (evitar ruído), **U**NIFY (a mesma coisa tem sempre o mesmo aspeto), **C**HECK (integridade visual);
  - composição: **C**ONDENSE (densidade), **E**XPRESS (escolher o gráfico certo), **S**TRUCTURE (organizar), **S**AY (dar uma mensagem).
- Âmbito: os gráficos matplotlib da secção 6 do notebook, as tabelas formatadas com `tabela_nova` e o AI/BI Dashboard. As tabelas de
  verificação do `display()` ficam de fora: não se podem estilizar.

### Decisões do grupo onde nos afastamos das IBCS

| Tema | IBCS | Nós | Porquê |
|---|---|---|---|
| Cor (SI 2.2, UN 5.1) | dados a preto/cinzento; cor só para realçar; verde/vermelho = bom/mau | **híbrido**: cada entidade mantém a sua cor da NOVA IMS ([ESTILO_VISUAL §3](ESTILO_VISUAL.md#3-cores-por-entidade)), mas em cada gráfico só a protagonista e 1–2 séries de comparação têm cor e o resto fica `NOVA_CINZA`. Verde e vermelho **nunca** significam bom/mau | a identidade visual da NOVA IMS é um requisito do grupo; manter a mesma cor por entidade em todos os gráficos é o próprio princípio UNIFY |
| Intervalos de tempo (UN 3.3) | `2000..2026` | `2000–2026` (travessão) | lê-se melhor em pt-PT |
| Título (UN 2.2) | medida a negrito, unidade normal | título todo a negrito (rcParams) | o matplotlib não mistura pesos num `set_title`; não compensa a complexidade |
| Grelha (SI 3.1) | substituir grelha e eixo de valores por rótulos de dados | grelha leve mantida nas séries densas (linhas, dispersão, mapa) | as próprias IBCS aceitam grelha em gráficos com muitos pontos |

## 2. Regras transversais

### 2.1 Simplificar (SIMPLIFY)
- Sem fundos coloridos, molduras, sombras nem 3D (**SI 1.2, SI 2.1**). Já garantido pelos rcParams do ESTILO_VISUAL.
- A cor só se usa com significado: identificar uma entidade ou realçar a mensagem. Nunca para decorar (**SI 2.2**).
- O negrito só serve para distinguir (títulos, totais); nada de itálicos decorativos (**SI 2.3**).
- Nos gráficos com poucos pontos (barras, colunas), os **rótulos de dados** substituem o eixo de valores e a grelha (**SI 3.1**).
  Nas séries densas mantém-se a grelha leve.
- Sem palavras óbvias ou redundantes: "evolução", "gráfico", "análise", "total de" (**SI 4.1, SI 4.2**). Sem repetir a mesma palavra em
  todas as categorias ou legendas (ex.: o ano em cada mês) (**SI 4.3**).
- No máximo **3 algarismos** num rótulo de gráfico e 4 numa tabela; arredondar e usar prefixos (k, m, b) (**SI 5.2**).
- Não rotular valores pequenos nem todos os pontos: só os extremos, o primeiro, o último ou o valor da mensagem (**SI 5.1, SI 5.3**).

### 2.2 Texto: títulos, mensagem, legendas, notas (UNIFY, SAY)
- **O título é descritivo** e não tem conclusões: diz **quem** (objeto), **o quê** (medida e unidade) e **quando** (período)
  (**UN 2.2**). Ex.: "Brent: preço por barril em EUR, médias mensais, 2000–2026".
- **A mensagem vem antes e separada do título** (**UN 2.1, SA 3.2**). No notebook, fica na célula de pergunta (antes) ou na nota
  "Como ler"/"Resposta" (depois) *(nosso)*. Uma frase completa, verificável ("A Galp é a única empresa com efeito positivo
  significativo"), não "resultados interessantes" (**SA 3.1, SA 4.2**).
- Num painel duplo, o que é comum aos dois vai no título da figura e o que muda vai no subtítulo de cada painel (**UN 2.2**).
- **Legendas integradas**: nas linhas, o nome da série à direita do último ponto ou junto à linha; na barra/coluna simples, a legenda
  vai no título (**UN 2.3**). Legenda externa só quando não houver alternativa.
- **Texto sempre na horizontal**: legendas, categorias, rótulos e ticks. Nada de rótulos a 90° (**UN 2.3, UN 3.4**).
- Rótulos de dados junto ao elemento: acima dos valores positivos e abaixo dos negativos (colunas, linhas); à direita dos positivos e
  à esquerda dos negativos (barras) (**UN 2.3**).
- **Notas de rodapé**: fonte dos dados e definições ("retornos com dividendos", "preços de 2026-09") vão em rodapé, 8 pt, a
  `NOVA_CINZA_TEXTO`, e não no título (**UN 2.5, SA 4.4**). A fonte (FRED, Yahoo Finance, OCDE via FRED) aparece sempre.
- Anotações explicativas ficam dentro do gráfico, ligadas ao ponto a que se referem (**UN 2.4, SA 4.5**).

### 2.3 Números, unidades e datas (UNIFY)
- Datas em **ISO 8601**: `2026-09` (mês), `2026-09-30` (dia), `2026` (ano). Em matplotlib: `f"{d:%Y-%m}"` (**UN 1.2, UN 3.3**).
- Moedas com o **código ISO 4217**: `EUR`, `USD`, nunca `€`/`$`, porque o projeto mistura duas moedas (**UN 1.2**).
  Ex.: "Preço por barril em EUR", "86 EUR reais".
- Prefixos: `k`, `m`, `b` em minúscula junto à moeda (`mEUR`) (**UN 1.2**).
- Sinal `+` só em **variações** (ex.: "+12 %"); negativos sempre com `-` (**UN 1.2**).
- Os termos são os mesmos em todo o lado: Brent (EUR), PSI, "ações PT (índice OCDE)", "cabaz sem Galp" (**UN 1.1**). Os nomes vêm de
  `sv_dim_instrument` e das constantes do notebook.

### 2.4 Dimensões e eixos (UNIFY)
- **O tempo vai no eixo horizontal**, da esquerda para a direita (**UN 3.3**).
- **Categorias de estrutura** (empresas, setores, episódios) vão no **eixo vertical**, como barras horizontais, com os nomes
  legíveis sem rodar (**UN 3.4**).
- Nos gráficos de barras/colunas, a largura transmite o tipo de medida: medidas absolutas a 2/3 da categoria e relativas (%, rácios) a
  1/3. Nas linhas, as medidas absolutas são grossas e as relativas finas (**UN 3.1**). *(nosso: aplicar só quando o gráfico
  mistura os dois tipos.)*
- Índices: a série relativa a uma base (100) marca o ponto de referência com uma **seta preta à esquerda do 100** e, se ajudar, uma
  linha horizontal (**UN 4.2, UN 5.1**).

### 2.5 Integridade visual (CHECK)
- **Os eixos de valor começam em zero** (**CH 1.1**). Exceções: índices (base 100) e grandezas com sinal (correlações, retornos), em
  que o 0 ou o 100 tem de estar visível e marcado com uma linha de referência.
- Sem escalas logarítmicas (**CH 1.2**), salvo justificação explícita na nota.
- **Sem barras cortadas** nem eixos partidos (**CH 2.1**). Um valor extremo e sem importância para a mensagem desenha-se até ao limite
  da escala com um **marcador de outlier** (triângulo a apontar no sentido do crescimento) e o valor verdadeiro em rótulo
  (**CH 4.4, UN 5.3**).
- **A mesma unidade tem a mesma escala** nos painéis lado a lado (`sharex`/`sharey`) (**CH 4.1, CO 5.1**). Se não der, usar uma faixa de
  escala (**UN 5.2, CH 4.3**).
- Histogramas com classes de igual largura (**CH 1.3**).
- Áreas e volumes não representam valores lineares; preferir barras (**CH 3.1, CH 3.2**).
- Séries longas mostram o efeito da **inflação** e da **moeda** (**CH 5.1, CH 5.2**). No projeto, o Brent em USD, em EUR nominais e em
  EUR reais.

### 2.6 Realçar a mensagem (SAY, UNIFY)
- Cada gráfico realça o que prova: a protagonista é grossa (2.5 pt) e tem cor; o valor-chave tem anotação ou elipse de realce
  (**SA 4.3, UN 5.1**). Um gráfico sem realce é estatística, não mensagem.
- Diferenças entre duas barras: linhas auxiliares e um marcador de diferença (**UN 5.1**). *(nosso: o marcador usa `NOVA_AZUL`, a cor
  IBCS para "neutro", e não verde nem vermelho.)*
- Tendências: seta do primeiro ao último período, com o valor e o método (ex.: "CAGR 4 %") (**UN 5.1**).

### 2.7 Escolher o gráfico (EXPRESS, CONDENSE)
- **Tabela** para consultar valores; **gráfico** para provar uma mensagem (**EX 1**).
- Evitar: **tartes e anéis** (**EX 2.1**), **gauges** (**EX 2.2**), **radar e funil** (**EX 2.3**), **semáforos** (**EX 2.5**).
  Usar barras.
- Mais de 3–4 linhas que se cruzam é um **gráfico esparguete**. Passa a ***small multiples***: um painel por série, com a mesma
  escala (**EX 2.4, CO 5.1**).
- Diagramas conceptuais (caixas e setas) só quando são essenciais; no projeto, só o diagrama da arquitetura (**EX 3.1**).
- Acrescentar comparações sempre que existam (outro período, a média do mercado, o cabaz) (**EX 4**). Correlações explicam-se com
  dispersões ou rankings (**EX 5.3**).
- Gráficos relacionados podem ficar na mesma figura, se ajudarem a ler a mensagem (**CO 5.2**).

## 3. Regras por tipo de gráfico

| Tipo | Quando usar | Eixos | Legenda e rótulos | Cor (híbrido) | matplotlib |
|---|---|---|---|---|---|
| **Linhas** | evolução no tempo com muitos pontos; ≤ 3 linhas que se cruzam (**EX 1.1, EX 2.4**) | tempo na horizontal; valor a partir de 0 (ou 0/100 marcado) | nome no fim de cada linha; rotular só extremos e último valor (**UN 2.3, SI 5.3**) | protagonista com cor e 2.5 pt; comparações com cor e 1.5 pt; restantes a cinzento | `ax.annotate(nome, (x[-1], y[-1]), xytext=(6, 0), textcoords="offset points")` |
| **Colunas** | poucos períodos (ex.: médias anuais); nunca linhas com poucos pontos (**EX 1.1**) | tempo na horizontal; base 0 | legenda no título; rótulo acima dos positivos e abaixo dos negativos; sem eixo de valores (**SI 3.1**) | uma cor (a da entidade); realce de 1 coluna se for a mensagem | `ax.bar(...)`, `ax.bar_label(barras, fmt="%.0f")`, `ax.yaxis.set_visible(False)` |
| **Barras simples** | comparar categorias de estrutura num período (**UN 3.4**) | categorias na vertical, ordenadas pelo critério da análise (valor ou grupo) | rótulo à direita (positivos) / à esquerda (negativos); nomes horizontais | cinzento; só a categoria da mensagem com cor | `ax.barh(...)`, `ax.bar_label(...)`, `ax.grid(axis="y", visible=False)` |
| **Barras agrupadas** | duas medidas ou cenários por categoria (**EX 1.1**) | como as barras simples; mesma escala nas duas séries | legenda integrada por cima da primeira categoria ou com linha auxiliar (**UN 2.3**) | as duas entidades com a sua cor | `ax.barh(pos ± 0.2, ...)` |
| **Pins** | variações relativas (%) por categoria ou período (**EX 1.1**) | como barras/colunas, traço fino com cabeça | rótulo na ponta | cor da entidade; verde/vermelho **não** (decisão do grupo) | `ax.hlines` + `ax.plot(marker="o")` |
| **Waterfall** | decompor uma diferença em contributos (**EX 1.1**) | base e total como barras inteiras; contributos flutuantes | rótulo em cada contributo; linhas auxiliares entre barras | base/total a preto; contributos a cinzento ou na cor da entidade | `ax.bar(bottom=...)` |
| **Dispersão** | relação entre duas medidas (correlação) (**EX 5.3**) | dois eixos de valor; 0 marcado nos dois | legenda junto da reta/pontos; rotular só os pontos referidos na mensagem | uma cor, alpha 0.6; a reta a preto | `ax.scatter`, `ax.axhline(0)`, `ax.axvline(0)` |
| **Índice base 100** | comparar trajetórias com escalas diferentes (**UN 4.2**) | tempo na horizontal; linha no 100 | nome e valor final no fim de cada linha; **seta de referência** "100" à esquerda | como nas linhas | `ax.annotate("100 ▶", ...)` à esquerda do 1.º ponto |
| ***Small multiples*** | > 3 séries cuja forma importa (**CO 5.1, EX 2.4**) | **mesma escala** em todos os painéis | nome da série no subtítulo de cada painel | uma cor por painel (a da entidade) | `plt.subplots(..., sharex=True, sharey=True)` |
| **Mapa de calor** | uma grandeza em tempo × estrutura (sem notação IBCS; derivado *(nosso)*) | tempo na horizontal, estrutura na vertical (**UN 3.3, 3.4**); rótulos horizontais | barra de cores com a unidade; nomes dos grupos ao lado | escala divergente laranja–azul do ESTILO_VISUAL, centrada em 0 | `ax.imshow(..., vmin=-v, vmax=v)`, `ax.set_xticklabels(..., rotation=0)` |
| **Tabela formatada** | consultar valores (**EX 1.2**) | tempo em colunas, estrutura em linhas (**UN 3.3**) | números alinhados à direita, sem linhas verticais (**SI 3.2**); ≤ 4 algarismos | realce de linha a verde claro (ESTILO_VISUAL §8) | `tabela_nova(...)` |

## 4. Avaliação dos gráficos atuais (13-11-2026)

Leitura dos gráficos matplotlib da secção 6 do notebook (as células são as do [ANALISES.md](ANALISES.md#análises-por-pergunta)).
As correções foram aplicadas a 13-11-2026 e verificadas numa execução local completa; falta vê-las no Databricks.

Ajudantes comuns, na célula de preparação (47):
- `rotular_fim(ax, itens)`: nome (e valor) de cada linha à direita do último ponto, afastando os rótulos sobrepostos (UN 2.3);
- `finalizar(fig, fonte)`: layout final com a nota de rodapé da fonte e das definições (UN 2.5, SA 4.4);
- `sombrear_crises(ax)`: faixas das crises com o nome na horizontal, em dois níveis (UN 2.3).

| Célula | Gráfico | Estado | O que foi feito (regra) |
|---|---|---|---|
| 49 | Brent em USD, EUR nominais e EUR reais (6.1) | cumpre | já tinha o eixo a partir de 0 e o efeito da moeda e da inflação (CH 1.1, CH 5.1, CH 5.2); legenda → nome no fim de cada linha (UN 2.3); `EUR` e datas `AAAA-MM` (UN 1.2); anotações acima do pico mais alto; fonte em rodapé (SA 4.4) |
| 53 | Dispersão Brent EUR × ações PT (6.2) | cumpre | título descritivo, com a correlação e o *n* passados para a legenda da reta, que fica por cima do gráfico (UN 2.2); fonte em rodapé |
| 57 | Correlação móvel PSI e cabaz (6.3) | cumpre | nomes das crises na horizontal (UN 2.3); legenda → nome no fim das linhas; período no título; fonte |
| 58 | Mapa de calor da correlação 36 m (6.3) | cumpre | anos na horizontal (UN 2.3); chave de cores passou do título para a barra de cores (UN 2.2); fonte |
| 61 | Efeito do petróleo por empresa, 2 painéis (6.4) | cumpre | cor só na Galp (protagonista) e no cabaz (comparação), o resto a cinzento, com os setores separados e nomeados à direita (SI 2.2, híbrido); painéis com a mesma escala (CH 4.1); título da figura com quem/o quê/quando (UN 2.2); a leitura das barras de erro passou para o rodapé |
| 65 | Correlação 36 m Galp vs cabaz (6.5) | cumpre | legenda duplicada retirada (SI 4); nomes das crises na horizontal; fonte |
| 66 | Base 100 real: Galp, cabaz, Brent (6.5) | cumpre | seta de referência "100 ▶" (UN 4.2); eixo a partir de 0; legenda duplicada retirada (SI 4); método ("com dividendos", preços reais) passou para o rodapé (UN 2.5); datas `AAAA-MM` |
| 70 | Variação por episódio: Brent vs ações PT (6.6) | cumpre | **sem barra cortada**: a barra de +285% vai até ao limite da escala com um triângulo de outlier e o valor real em rótulo (CH 2.1, CH 4.4, UN 5.3); rótulos de dados no lugar do eixo e da grelha (SI 3.1); legenda por cima do gráfico; unidade no título; fonte |
| 3 | Diagrama da arquitetura | cumpre | único diagrama conceptual (EX 3.1) |

As notas "Como ler" das respostas 1, 4, 5 e 6 foram ajustadas às novas cores, unidades (`EUR`) e datas.

## 5. Checklist de revisão

Antes de aceitar um gráfico novo ou alterado, confirmar:

- [ ] **Tipo certo** para a pergunta: tempo → linhas/colunas; estrutura → barras; relação → dispersão. Sem tarte, gauge, radar ou
  semáforo (EX 1.1, EX 2).
- [ ] **Tempo na horizontal, estrutura na vertical**, com todo o texto na horizontal (UN 3.3, UN 3.4, UN 2.3).
- [ ] **Título descritivo** (quem, o quê com a unidade, quando), sem conclusão; a mensagem está na célula de texto (UN 2.2, SA 3.2).
- [ ] **Legenda integrada** (fim da linha, título ou junto às barras), sem duplicar rótulos (UN 2.3, SI 4).
- [ ] **Eixo de valores a partir de 0**, ou 0/100 visível e marcado; sem escala log (CH 1.1, CH 1.2).
- [ ] **Sem barras cortadas**; outliers com marcador e o valor real (CH 2.1, CH 4.4).
- [ ] **Mesma escala** para a mesma unidade em painéis lado a lado (CH 4.1).
- [ ] **Cor híbrida**: cores de entidade do ESTILO_VISUAL; só a protagonista e 1–2 comparações em cor; verde/vermelho não
  significam bom/mau (SI 2.2).
- [ ] **≤ 3–4 linhas** que se cruzam; acima disso, *small multiples* (EX 2.4).
- [ ] **Rótulos** só onde ajudam (extremos, último valor, valor da mensagem), com ≤ 3 algarismos (SI 5).
- [ ] **Formatos ISO**: `2026-09`, `EUR`/`USD`, `+` só em variações (UN 1.2).
- [ ] **Mensagem realçada** (protagonista, anotação ou elipse) e **fonte** em rodapé (SA 4.3, SA 4.4).

## 6. Atualizar este documento

- Um gráfico novo entra na tabela da [secção 4](#4-avaliação-dos-gráficos-atuais-13-11-2026) (e no [ANALISES.md](ANALISES.md)).
- Quando um gráfico muda, a sua linha é revista (estado e o que foi feito).
- Um desvio novo às IBCS é registado em [Decisões do grupo](#decisões-do-grupo-onde-nos-afastamos-das-ibcs), com o porquê, **antes** de
  mudar o gráfico.

## 7. Regra obrigatória

**Toda a alteração a um gráfico (novo, alterado ou do dashboard) segue e revê as boas práticas IBCS deste documento**, incluindo a
[checklist](#5-checklist-de-revisão). Em caso de dúvida, confirmar a regra no PDF IBCS pelo seu ID. Esta regra soma-se à do
[ESTILO_VISUAL.md](ESTILO_VISUAL.md#10-regra-obrigatória) (cores e fontes) e está também no [CLAUDE.md](../CLAUDE.md) (secções 2, 4 e 5).
