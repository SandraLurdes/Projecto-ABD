# Estilo visual (NOVA IMS)

Paleta, tipografia e *look & feel* dos gráficos, tabelas e diagramas do projeto, segundo a identidade da NOVA IMS.
**Todos os gráficos e tabelas formatadas do notebook seguem este documento** (ver a [regra obrigatória](#10-regra-obrigatória) no fim).

## 1. Origem

As cores e as fontes vêm das páginas *Color Scheme* e *Typography* do guia de apresentações da NOVA IMS, partilhadas pelo grupo.
Os códigos hexadecimais foram amostrados pixel a pixel nessas imagens.

[FLAG] Não são os valores publicados no manual da marca. Se o grupo tiver o manual oficial, confirmar os hex e corrigir aqui (e só aqui).

## 2. Paleta

| Papel na NOVA IMS | Nome no código | Hex | Uso |
|---|---|---|---|
| Texto principal | `NOVA_TEXTO` | `#000000` | todo o texto (títulos, eixos, rótulos) |
| #1 Cor principal | `NOVA_VERDE` | `#67DD7D` | a série protagonista do gráfico |
| #2 Cor secundária | `NOVA_AZUL` | `#4875F6` | a segunda série mais importante |
| #3 Acento | `NOVA_ROSA` | `#EA3399` | acento |
| #4 Acento | `NOVA_AMARELO` | `#F1DB49` | acento, só em áreas (barras, faixas), nunca em linhas finas nem texto |
| #5 Acento | `NOVA_LARANJA` | `#EB512D` | acento; polo negativo do mapa divergente |
| #6 Acento | `NOVA_PRETO` | `#000000` | acento (linhas de referência, uma série neutra forte) |

Neutros auxiliares (não fazem parte do guia da NOVA IMS; servem a estrutura do gráfico, não os dados):

| Nome no código | Hex | Uso |
|---|---|---|
| `NOVA_CINZA` | `#8C8C8C` | série de comparação/neutra ("os outros", valores nominais) |
| `NOVA_CINZA_TEXTO` | `#595959` | texto secundário: anotações, notas, ticks, rótulos dos eixos |
| `NOVA_CINZA_EIXO` | `#BFBFBF` | linhas dos eixos, barras de erro |
| `NOVA_CINZA_GRELHA` | `#E6E6E6` | grelha, faixas de crises |
| `NOVA_CINZA_FUNDO` | `#F2F2F2` | linhas alternadas das tabelas, centro do mapa divergente |
| `NOVA_FUNDO` | `#FFFFFF` | fundo de figuras e tabelas |

Regras de uso:
- O **verde** é a cor da marca: cabe à série principal (protagonista) de cada gráfico. O **azul** é a segunda cor.
- Rosa e preto são as cores seguintes, quando há mais séries. O amarelo só serve para áreas. O laranja está reservado para a Galp e
  para o polo negativo da escala divergente ([secção 3](#3-cores-por-papel-e-a-exceção-da-galp)).
- O texto é sempre preto ou cinzento. Nunca se escreve texto a cores (o verde e o amarelo têm pouco contraste sobre branco).
- Nunca se usam hex soltos num gráfico: usam-se as constantes da [secção 7](#7-código-de-referência).

## 3. Cores por papel (e a exceção da Galp)

A cor de uma série depende do **papel que tem no gráfico**, não da entidade. Em cada gráfico, as séries ordenam-se por importância e
recebem as cores da hierarquia (`HIERARQUIA`), por esta ordem:

| Ordem | Cor | Constante |
|---|---|---|
| 1.ª (protagonista, a linha mais grossa) | verde `#67DD7D` | `HIERARQUIA[0]` |
| 2.ª | azul `#4875F6` | `HIERARQUIA[1]` |
| 3.ª | rosa `#EA3399` | `HIERARQUIA[2]` |
| 4.ª | preto `#000000` | `HIERARQUIA[3]` |
| contexto ("os outros") | cinzento `#8C8C8C` | `NOVA_CINZA` |

**Exceção da Galp:** a Galp é **sempre laranja** `#EB512D` (`C_GALP`, a cor da marca Galp), seja qual for o seu papel. Fica fora da
hierarquia e não ocupa lugar nela: as outras séries continuam a receber verde, azul, rosa… pela sua ordem. Exemplo: Galp + cabaz →
Galp laranja, cabaz verde; Galp + cabaz + Brent → laranja, verde, azul.

No código, as cores atribuem-se com `cores_por_ordem(*series)`, que aplica a hierarquia e a exceção
([secção 7](#7-código-de-referência)). Uma entidade pode ter cores diferentes em gráficos diferentes; por isso as linhas levam rótulo
direto e cada "Como ler" diz as cores daquele gráfico. Dentro da mesma figura (vários painéis), a mesma série tem a mesma cor.

Aplicação atual (secção 6 do notebook):

| Gráfico | Verde (1.ª) | Azul (2.ª) | Rosa (3.ª) | Laranja (Galp) | Cinzento |
|---|---|---|---|---|---|
| 6.1 câmbio | Brent em USD | Brent em EUR | taxa EUR/USD | | |
| 6.1 inflação | Brent em euros de 2000 | Brent em EUR (nominal) | | | |
| 6.2 dispersão | mercado e setores | | | painel da Galp | |
| 6.3 correlação móvel | cabaz sem Galp | PSI | | | |
| 6.4 efeito por empresa | cabaz sem Galp | | | Galp | as outras empresas |
| 6.5 correlação Galp vs. cabaz | cabaz sem Galp | | | Galp | |
| 6.5 reação ao Brent | cabaz sem Galp | | | Galp | |
| 6.6 episódios | Brent (EUR) | ações PT | | | |

Os setores não têm cor: servem só para ordenar e agrupar as empresas (`SECTOR_ORDER`).

Escalas:
- **Divergente** (correlações, de −1 a +1): laranja `#EB512D` → `#F2F2F2` → azul `#4875F6`. Valores em falta a branco.
- **Sequencial** (uma só grandeza, do menos ao mais): `#F2F2F2` → verde `#67DD7D` → `#1F6B33` (verde escuro, para o extremo ter contraste).

## 4. Acessibilidade

A paleta da marca **não foi pensada para daltonismo**: verde, laranja e rosa confundem-se para quem não distingue vermelho/verde.
Por isso:
- num mesmo gráfico, a cor nunca é a única forma de distinguir séries: rotular as linhas diretamente no fim (como já se faz com
  `ax.annotate`) e variar a espessura (protagonista mais grossa) ou o estilo (`--` para a série de comparação);
- o par laranja–azul é o único seguro para codificar "negativo vs positivo", por isso é o do mapa divergente;
- nos gráficos de barras por empresa, as empresas ficam agrupadas por setor, com separadores e o nome do setor ao lado do grupo; só a
  Galp e o cabaz levam cor (ver [IBCS_GRAFICOS.md](IBCS_GRAFICOS.md)).

## 5. Tipografia

| Uso | NOVA IMS | Nos gráficos (matplotlib) |
|---|---|---|
| Títulos | **Boma** | Arial **negrito** |
| Corpo | Arial | Arial |

A Boma não está instalada nem existe no Databricks, por isso fica só para os slides da apresentação. Nos gráficos usa-se a pilha
`["Arial", "Liberation Sans", "DejaVu Sans"]`: o compute do Databricks é Linux e não tem Arial, e a Liberation Sans tem as mesmas
medidas. A primeira fonte disponível é a usada, sem avisos.

Tamanhos (pontos):

| Elemento | Tamanho | Peso / cor |
|---|---|---|
| Título do gráfico | 13 | negrito, preto, alinhado à esquerda |
| Título da figura (`suptitle`) | 14 | negrito, preto |
| Rótulos dos eixos | 10 | normal, `NOVA_CINZA_TEXTO` |
| Ticks | 9 | normal, `NOVA_CINZA_TEXTO` |
| Legenda e anotações | 9 | normal, preto (anotações de apoio a cinzento) |
| Nota de rodapé / fonte | 8 | normal, `NOVA_CINZA_TEXTO` |

Os títulos escrevem-se em frase ("Brent 2000–2026: …"), nunca em maiúsculas.

## 6. *Look & feel*

- Fundo branco, sem moldura à volta da figura.
- Sem os eixos de cima e da direita; eixos restantes finos, em `NOVA_CINZA_EIXO`.
- Grelha leve (`NOVA_CINZA_GRELHA`, 0.6 pt). Nos gráficos de barras horizontais, só a grelha vertical.
- Linhas: 1.5 pt para as séries normais, **2.5 pt para a protagonista**. Pontos de dispersão com `alpha` 0.6.
- Linhas de referência (zero, base 100) a preto, 0.8 pt.
- Faixas de crises em `NOVA_CINZA_GRELHA`, com o nome a `NOVA_CINZA_TEXTO`, 8 pt, na horizontal (em dois níveis).
- Legendas sem moldura; sempre que couber, rótulo direto no fim da linha em vez de legenda.
- Um gráfico responde a uma pergunta: a protagonista e no máximo 1–2 séries de comparação têm cor; o resto vai a `NOVA_CINZA`
  (regra híbrida acordada com as IBCS, ver [IBCS_GRAFICOS.md](IBCS_GRAFICOS.md#decisões-do-grupo-onde-nos-afastamos-das-ibcs)).
- O verde e o vermelho/laranja nunca significam "bom" ou "mau": o verde marca a protagonista e o laranja a Galp (ou o polo negativo
  da escala divergente).

## 7. Código de referência

Aplicado (13-11-2026) na célula de preparação dos gráficos da secção 6 do notebook, que define estas constantes para todos os gráficos.
Cada gráfico obtém as cores das suas séries com `cores_por_ordem(...)`, pela ordem de importância (ver a [secção 3](#3-cores-por-papel-e-a-exceção-da-galp)).
O mapa de calor da 6.3 usa `CMAP_DIVERGENTE`; as notas "Como ler" descrevem as cores de cada gráfico por estes nomes (verde, azul, rosa, laranja).

```python
# Estilo visual NOVA IMS (docs/ESTILO_VISUAL.md): paleta, tipografia e aspeto comuns a todos os gráficos
import matplotlib.pyplot as plt
from matplotlib.colors import LinearSegmentedColormap

NOVA_TEXTO, NOVA_VERDE, NOVA_AZUL = "#000000", "#67DD7D", "#4875F6"
NOVA_ROSA, NOVA_AMARELO, NOVA_LARANJA, NOVA_PRETO = "#EA3399", "#F1DB49", "#EB512D", "#000000"
NOVA_CINZA, NOVA_CINZA_TEXTO, NOVA_CINZA_EIXO = "#8C8C8C", "#595959", "#BFBFBF"
NOVA_CINZA_GRELHA, NOVA_CINZA_FUNDO, NOVA_FUNDO = "#E6E6E6", "#F2F2F2", "#FFFFFF"

# Cores por papel em cada gráfico: protagonista verde, 2.ª série azul, depois rosa e preto; o contexto a cinzento.
# A Galp é a exceção: sempre laranja (a cor da marca Galp), fora da hierarquia, que continua igual para as outras séries
HIERARQUIA = [NOVA_VERDE, NOVA_AZUL, NOVA_ROSA, NOVA_PRETO]
C_GALP = NOVA_LARANJA

def cores_por_ordem(*series):
    """Cor de cada série, pela ordem de importância no gráfico; a Galp leva C_GALP e não ocupa lugar na hierarquia."""
    resto = iter(HIERARQUIA)
    return [C_GALP if s in ("Galp", "GALP.LS") else next(resto) for s in series]

SECTOR_ORDER = ["Petróleo e gás", "Eletricidade e redes", "Indústria exportadora", "Consumo e retalho",
                "Serviços domésticos", "Exposição a países petrolíferos", "Cabaz sem Galp"]

CMAP_DIVERGENTE = LinearSegmentedColormap.from_list("nova_div", [NOVA_LARANJA, NOVA_CINZA_FUNDO, NOVA_AZUL])
CMAP_DIVERGENTE.set_bad(NOVA_FUNDO)
CMAP_SEQUENCIAL = LinearSegmentedColormap.from_list("nova_seq", [NOVA_CINZA_FUNDO, NOVA_VERDE, "#1F6B33"])

plt.rcParams.update({
    "font.family": "sans-serif", "font.sans-serif": ["Arial", "Liberation Sans", "DejaVu Sans"],
    "text.color": NOVA_TEXTO, "figure.facecolor": NOVA_FUNDO, "axes.facecolor": NOVA_FUNDO,
    "axes.spines.top": False, "axes.spines.right": False, "axes.edgecolor": NOVA_CINZA_EIXO, "axes.linewidth": 0.8,
    "axes.grid": True, "axes.axisbelow": True, "grid.color": NOVA_CINZA_GRELHA, "grid.linewidth": 0.6,
    "axes.titlesize": 13, "axes.titleweight": "bold", "axes.titlelocation": "left", "axes.titlecolor": NOVA_TEXTO,
    "figure.titlesize": 14, "figure.titleweight": "bold",
    "axes.labelsize": 10, "axes.labelcolor": NOVA_CINZA_TEXTO,
    "xtick.labelsize": 9, "ytick.labelsize": 9, "xtick.color": NOVA_CINZA_TEXTO, "ytick.color": NOVA_CINZA_TEXTO,
    "legend.fontsize": 9, "legend.frameon": False,
    "lines.linewidth": 1.5, "axes.prop_cycle": plt.cycler(color=HIERARQUIA),
})
```

Nos gráficos, a protagonista leva `lw=2.5`; as linhas de referência `color=NOVA_PRETO, lw=0.8`; as faixas de crises
`color=NOVA_CINZA_GRELHA`; as barras de erro `ecolor=NOVA_CINZA_TEXTO`.

## 8. Tabelas

- O `display()` de um DataFrame Spark é desenhado pelo Databricks e **não se pode estilizar**: as tabelas de verificação e de
  controlo ficam assim.
- As tabelas que vão para a apresentação ou que têm de sair formatadas no HTML exportado passam a pandas e usam um `Styler` (a função `tabela_nova` está definida na célula de preparação da secção 6 e é usada nas tabelas de resultados da secção 6: correlações (6.2), veredictos (6.4), episódios (6.6) e modelos de ML (6.7)):

```python
# Tabela no estilo NOVA IMS (para tabelas pequenas de resultados)
def tabela_nova(pdf, destacar=None, casas=2):
    """destacar: nome de uma coluna booleana cujas linhas ficam realçadas a verde claro."""
    sty = (pdf.style.format(precision=casas).hide(axis="index")
           .set_table_styles([
               {"selector": "th", "props": f"background-color: {NOVA_PRETO}; color: {NOVA_FUNDO}; font-family: Arial; font-weight: bold; padding: 6px 10px;"},
               {"selector": "td", "props": f"font-family: Arial; padding: 4px 10px; border-bottom: 1px solid {NOVA_CINZA_GRELHA};"},
               {"selector": "tbody tr:nth-child(even)", "props": f"background-color: {NOVA_CINZA_FUNDO};"},
           ])
           .set_properties(subset=pdf.select_dtypes("number").columns, **{"text-align": "right"}))
    if destacar:
        sty = sty.apply(lambda r: ["background-color: #D9F6DF" if r[destacar] else "" for _ in r], axis=1)
    return sty
```

- Cabeçalho preto com texto branco, linhas alternadas `NOVA_CINZA_FUNDO`, números à direita, texto à esquerda.
- Realce de linhas importantes com verde claro `#D9F6DF` (o verde da marca a 25 %, para o texto preto continuar legível).

**AI/BI Dashboard:** nas definições de cada widget, aplicar a mesma regra da [secção 3](#3-cores-por-papel-e-a-exceção-da-galp):
a série principal do widget a verde, a segunda a azul, depois rosa e preto; a Galp sempre a laranja; a escala divergente laranja–azul nas correlações.

## 9. Diagramas

O diagrama da arquitetura (`img/arquitetura_medallion.mmd`) mantém as cores próprias das camadas (bronze, prata, ouro), que já têm
um significado. O texto dos nós passa a preto, as setas a `NOVA_CINZA_TEXTO`, e as caixas de fonte e análise podem usar o verde e o
azul da NOVA IMS clareados. Depois de mudar o `.mmd`, regenerar o PNG (ver [ARQUITETURA.md](ARQUITETURA.md#1-visão-geral)).

## 10. Regra obrigatória

**Qualquer gráfico, tabela formatada ou diagrama do projeto, novo ou alterado, segue este documento.**
- As cores vêm das constantes da [secção 7](#7-código-de-referência), nunca de hex escritos no gráfico.
- As cores das séries seguem a hierarquia por papel da [secção 3](#3-cores-por-papel-e-a-exceção-da-galp) (`cores_por_ordem`), com a Galp
  sempre a laranja; um gráfico novo ou alterado entra na tabela "Aplicação atual" dessa secção.
- Se uma alteração precisar de fugir a estas regras, muda-se primeiro este documento e só depois o gráfico.
- **Além das cores e fontes, todo o gráfico novo ou alterado segue e revê as boas práticas IBCS de [IBCS_GRAFICOS.md](IBCS_GRAFICOS.md)**
  (tipo de gráfico, títulos, eixos, rótulos, escalas), incluindo a checklist da secção 5.

Esta regra também está no [CLAUDE.md](../CLAUDE.md) (secções 2, 4 e 5), por isso o assistente aplica-a automaticamente em todas as sessões.
