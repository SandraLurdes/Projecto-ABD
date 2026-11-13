# Petróleo vs. Bolsa Portuguesa (2000–2026): um Data Lakehouse com Apache Spark

Projeto de grupo da unidade curricular **Analyzing Big Data** (EDSA).

**Elementos do grupo**
- Ana Filipa Filipe — 20250313
- José Justino — 20251110
- Inês Rodrigues — 20251957
- Sandra Figueiredo — 20251705

**Plataforma:** Apache Spark no **Databricks Free Edition** (compute serverless, Unity Catalog, Delta Lake)

---

## Objetivo

Criar um processo de análise de dados **estruturado e incremental**, que replique uma situação real, para estudar a relação entre o **preço do petróleo** (Brent e WTI) e a **bolsa portuguesa** (índice PSI e 15 empresas cotadas), de 2000 até hoje.

### Hipótese do grupo
> **Quando o preço do petróleo sobe, a cotação das empresas portuguesas desce — exceto a Galp.**
> Para a Galp o petróleo é receita; para as outras empresas é um custo.

### Perguntas de análise
1. O Brent está hoje mais caro do que em 2000, em dólares, em euros e descontada a inflação?
2. A bolsa portuguesa desce quando o Brent sobe?
3. A ligação entre a bolsa portuguesa e o Brent muda ao longo do tempo?
4. O Brent penaliza as empresas portuguesas, com exceção da Galp?
5. A Galp reage ao Brent de forma diferente das restantes empresas?
6. Nas crises de 2008, da COVID-19 e da guerra na Ucrânia, a bolsa portuguesa acompanhou o Brent?
7. O Brent explica os movimentos diários do PSI e permite antecipá-los?

As empresas, o sinal esperado por setor e as regras de análise estão em [docs/METODOLOGIA.md](docs/METODOLOGIA.md).

---

## Estrutura do repositório

```
Projecto-ABD/
├── Lakehouse_Petroleo_PSI.ipynb   # entregável: notebook importado no Databricks
├── dados_fallback/                # 22 ficheiros de contingência com o formato das APIs (cópia de 07-11-2026; Euro Stoxx 50 de 13-11-2026), só usados se a API falhar
│   ├── fred_<série>.csv           # 5 séries FRED (Brent, WTI, EUR/USD, índice OCDE ações PT, IHPC Portugal)
│   └── yahoo_<símbolo>.json       # 17 séries Yahoo (PSI, Euro Stoxx 50 e 15 empresas)
├── docs/
│   ├── FONTES.md                  # fontes externas, pedidos às APIs e schema das respostas em bruto
│   ├── ARQUITETURA.md             # processo medallion, leituras incrementais, reprocessamento, mapa do notebook
│   ├── TABELAS.md                 # dicionário de dados das tabelas (controlo, bronze, silver, gold) e linhagem
│   ├── METODOLOGIA.md             # hipótese, empresas e setores, regras estatísticas, limitações
│   ├── ANALISES.md                # queries, gráficos, dashboard e modelos de ML
│   ├── ESTILO_VISUAL.md           # paleta, tipografia e aspeto NOVA IMS dos gráficos e tabelas
│   ├── IBCS_GRAFICOS.md           # boas práticas IBCS: escolha do gráfico, títulos, eixos, rótulos e checklist
│   ├── TOM_ESCRITA.md             # tom e regras de escrita do texto do notebook (apresentação)
│   ├── CONCLUSOES.md              # resultados e conclusões
│   ├── ESTADO.md                  # checklist e prazos
│   ├── DESENVOLVIMENTO.md         # correr e testar fora do Databricks
│   └── MANUAL_ABD.md              # manual de estudo da cadeira (teoria: lakehouse, Spark, Delta)
├── img/                           # diagrama da arquitetura (fonte Mermaid .mmd + PNG mostrado no notebook)
├── Project_Instructions.md        # enunciado do professor (aula 7 + regras de entrega e apresentação)
├── CLAUDE.md                      # instruções para o assistente Claude Code (não é preciso para ler o projeto)
├── requirements.txt               # bibliotecas usadas (já incluídas no Databricks)
├── requirements-local.txt         # bibliotecas para correr localmente (PySpark + Jupyter)
├── .gitignore                     # exclui o .venv/, o lakehouse local (lakehouse_oil_psi/) e o PDF da norma IBCS
└── README.md
```

---

## Fontes de dados

São 22 séries gratuitas e sem chave:
- **FRED:** Brent, WTI, EUR/USD, o índice OCDE de ações portuguesas e o IHPC de Portugal;
- **Yahoo Finance:** o índice PSI, o Euro Stoxx 50 (bolsa europeia) e 15 empresas cotadas.

Não existe PSI diário gratuito antes de 2013; por isso usam-se também o índice mensal da OCDE e as cotações das empresas.
Como o Databricks Free Edition restringe o acesso à internet, o notebook usa ficheiros de *fallback* (uma cópia antiga das respostas) quando a API falha. A fonte principal é sempre a API.
→ Detalhe e schemas das respostas: [docs/FONTES.md](docs/FONTES.md).

## Arquitetura (Medallion)

```
APIs ─► LANDING (ficheiros em bruto) ─► BRONZE (append-only, linhagem) ─► SILVER (normalizada, MERGE) ─► GOLD (analítica, otimizada)
                                                                                                          └─► Spark SQL · matplotlib · AI/BI Dashboard · MLlib
```

Há 2 leituras (carga histórica + 1 incremental, com 45 dias de sobreposição). A Silver pode ser reprocessada de raiz a partir
da Bronze, e o Delta Lake garante `MERGE`, *time travel* e `OPTIMIZE … ZORDER`.
→ Processo: [docs/ARQUITETURA.md](docs/ARQUITETURA.md) · Tabelas e colunas: [docs/TABELAS.md](docs/TABELAS.md).

---

## Como executar

O notebook corre em dois ambientes. O **Databricks** é o oficial: é lá que se faz a execução final e se exporta o HTML para a entrega.
A execução **local** serve só para testar alterações.

### Databricks vs local

| | Databricks Free Edition (oficial) | Local (testes) |
|---|---|---|
| Spark | o do runtime (serverless), nada a instalar | `pyspark` instalado com pip + Java 17 |
| Tabelas | Delta Lake + Unity Catalog (`workspace.oil_psi`) | Parquet em `./lakehouse_oil_psi/warehouse/oil_psi.db/` (ignorada pelo git) |
| Ficheiros (landing, fallback) | Volume `/Volumes/workspace/oil_psi/lake` | pasta `./lakehouse_oil_psi/` (ignorada pelo git) |
| Fallback | copiado automaticamente de `dados_fallback/` (Git folder); senão, *upload* manual | copiado automaticamente de `dados_fallback/` |
| Células `%sql` | nativas | convertidas automaticamente em `spark.sql(...)` |
| `display()` | tabelas e gráficos interativos | tabela em texto |
| `MERGE`, `OPTIMIZE … ZORDER`, *time travel* | sim (Delta) | o `MERGE` passa a *anti-join + union*; os outros são saltados |
| Internet | só domínios "confiáveis": as APIs bloqueadas leem o fallback | as duas APIs respondem: dados mais recentes, números diferentes de [CONCLUSOES.md](docs/CONCLUSOES.md) |
| Entrega em HTML | sim (*File → Export → HTML*) | não serve para a entrega |

O notebook deteta sozinho o ambiente (`IS_DATABRICKS`). O mesmo ficheiro corre nos dois sem alterações.

### A. Databricks Free Edition (execução oficial)

As bibliotecas usadas (`pyspark`, `requests`, `pandas`, `numpy`, `matplotlib`) já vêm instaladas no Databricks; o [requirements.txt](requirements.txt) serve apenas como documentação.

1. Abrir o repositório como **Git folder** (*Workspace → Create → Git folder*, com o URL do GitHub), ou atualizá-lo com *Pull*.
   Também se pode importar só o notebook (*Workspace → Import*), mas então o fallback não é copiado sozinho (ver passo 3).
2. Correr as células da secção **0. Configuração do ambiente**. Criam o schema `workspace.oil_psi` e o Volume `lake` e copiam os
   22 ficheiros de fallback de `dados_fallback/` para `/Volumes/workspace/oil_psi/lake/fallback/`. A cópia é idempotente: só copia o que falta.
3. *Só se o notebook foi importado sem a Git folder:* em **Catalog → workspace → oil_psi → Volumes → lake**, criar a pasta `fallback` e fazer *upload* dos **22 ficheiros** de `dados_fallback/`.
4. Correr **Run all**. Com `RESET = True` todas as tabelas e a landing são apagadas e reconstruídas (o fallback é mantido).
5. Criar o **AI/BI Dashboard** (*New → Dashboard*) sobre as tabelas `gd_*` (widgets propostos na introdução da secção 6 do notebook; especificação em [docs/ANALISES.md](docs/ANALISES.md)).
6. Exportar o notebook em **HTML** (*File → Export → HTML*) para a entrega.

### B. Local (só para testes)

**1. Instalar o Python 3.12 e o Java 17** (uma vez por computador):

| macOS (Terminal) | Windows (PowerShell) |
|---|---|
| `brew install python@3.12 openjdk@17` | `winget install Python.Python.3.12 EclipseAdoptium.Temurin.17.JDK` |

**2. Preparar o ambiente** (uma vez, na pasta do repositório):

macOS:
```bash
python3.12 -m venv .venv
.venv/bin/pip install -r requirements-local.txt
.venv/bin/python -m ipykernel install --user --name projecto-abd --display-name "Projecto ABD (PySpark local)"
```

Windows:
```powershell
py -3.12 -m venv .venv
.venv\Scripts\pip install -r requirements-local.txt
.venv\Scripts\python -m ipykernel install --user --name projecto-abd --display-name "Projecto ABD (PySpark local)"
```

**3. Correr:**
1. Abrir `Lakehouse_Petroleo_PSI.ipynb` no VS Code ou no Cursor.
2. Em **Select Kernel → Jupyter Kernel…**, escolher **"Projecto ABD (PySpark local)"**. Os outros kernels não têm o PySpark.
   Se este não aparecer, recarregar o editor (*Cmd/Ctrl+Shift+P → Developer: Reload Window*).
3. Clicar em **Run all**. O notebook encontra o Java, copia o fallback e corre as células `%sql` sozinho.

> **Windows:** para gravar as tabelas, o Spark precisa do `winutils.exe` + `hadoop.dll` (Hadoop 3) em `C:\hadoop\bin`, com
> `HADOOP_HOME=C:\hadoop`.

Outros erros: [docs/DESENVOLVIMENTO.md](docs/DESENVOLVIMENTO.md#problemas-comuns).

---|---|---|
| 1. Java 17 | `brew install openjdk@17` | `winget install EclipseAdoptium.Temurin.17.JDK` |
| 2. Ambiente Python | `python3.12 -m venv .venv`<br>`source .venv/bin/activate` | `py -3.12 -m venv .venv`<br>`.venv\Scripts\Activate.ps1` |
| 3. Bibliotecas | `pip install -r requirements-local.txt` | `pip install -r requirements-local.txt` |
| 4. Kernel Jupyter | `python -m ipykernel install --user --name projecto-abd --display-name "Projecto ABD (PySpark local)"` | igual |
| 5. *Só Windows*, para gravar tabelas | — | `winutils.exe` + `hadoop.dll` (Hadoop 3) numa pasta `...\hadoop\bin` e `HADOOP_HOME` a apontar para `...\hadoop` |

Também se pode usar o `uv` em vez do `venv`/`pip`: `uv venv .venv --python 3.12` e `uv pip install --python .venv -r requirements-local.txt`.

**Correr:**
1. Abrir `Lakehouse_Petroleo_PSI.ipynb` no VS Code ou no Jupyter e escolher o kernel **"Projecto ABD (PySpark local)"**. O kernel
   "Python 3" do Anaconda/sistema não tem o PySpark.
2. Correr **Run all**. O notebook trata sozinho do resto:
   - encontra o Java, se o `JAVA_HOME` não estiver definido;
   - copia o fallback de `dados_fallback/` para `./lakehouse_oil_psi/fallback/`;
   - corre as células `%sql`.
3. Em alternativa, pela linha de comandos:
   `jupyter nbconvert --to notebook --execute --ExecutePreprocessor.kernel_name=projecto-abd Lakehouse_Petroleo_PSI.ipynb --output run_local.ipynb`

Problemas comuns e detalhes: [docs/DESENVOLVIMENTO.md](docs/DESENVOLVIMENTO.md).

---

## Principais resultados

_Execução local de 13-11-2026 com as APIs (PySpark 4.3). Os números devem ser confirmados depois da execução no Databricks._

- **Galp: confirma a hipótese.** Num mês em que o Brent (EUR) sobe 10%, a Galp sobe em média +2,7% (t = 7,1), e +2,1% descontando a bolsa portuguesa (t = 5,7).
- **As outras 14 não confirmam no efeito total.** Todas têm correlação positiva com o petróleo e o cabaz sem a Galp sobe +1,0% por cada +10% no Brent (t = 4,2): petróleo e bolsa seguem o mesmo ciclo económico.
- **Descontando a bolsa portuguesa**, 7 das 14 ficam negativas, mas só a REN e os CTT de forma significativa. Descontando a bolsa europeia, nenhuma desce de forma significativa.
- **A relação depende do tipo de choque:** juntos nas crises de procura (correlação de 36 meses do cabaz sem a Galp em 0,73 no fim de 2020); na subida do Brent de 2026 caiu para −0,34, enquanto a Galp ficou em +0,50.
- **O petróleo explica pouco do PSI e não o antecipa:** R² de 7,3% em 2013–2023, negativo em 2024–2026, e um Random Forest abaixo da regra ingénua "sobe sempre" (AUC 0,44).

→ Todos os resultados e o mapa das conclusões do notebook: [docs/CONCLUSOES.md](docs/CONCLUSOES.md).

## Estado do projeto

Checklist e prazos (entrega até 16-11-2026, 23:59; apresentação a 17 ou 19-11-2026): [docs/ESTADO.md](docs/ESTADO.md).
