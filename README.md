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
1. Como evoluiu o preço do petróleo desde 2000, em dólares, em euros e em euros a preços de hoje (descontando a inflação)?
2. O mercado acionista português acompanha o petróleo? Em que períodos a relação é mais forte?
3. Como reage cada empresa ao petróleo, mês a mês? A Galp comporta-se de forma diferente das restantes? E depois de descontar o movimento geral da bolsa?
4. Como se comportaram os mercados nas grandes crises (2008, 2014-16, COVID-19, guerra na Ucrânia)?
5. Os movimentos do petróleo ajudam a explicar ou a prever os movimentos do PSI?

As empresas, o sinal esperado por setor e as regras de análise estão em [docs/METODOLOGIA.md](docs/METODOLOGIA.md).

---

## Estrutura do repositório

```
Projecto-ABD/
├── Lakehouse_Petroleo_PSI.ipynb   # entregável: notebook importado no Databricks
├── dados_fonte/                   # 21 ficheiros de contingência (seed) com o formato das APIs (snapshot de 07-11-2026)
│   ├── fred_<série>.csv           # 5 séries FRED (Brent, WTI, EUR/USD, índice OCDE ações PT, IHPC Portugal)
│   └── yahoo_<símbolo>.json       # 16 séries Yahoo (PSI + 15 empresas)
├── docs/
│   ├── FONTES.md                  # fontes externas, pedidos às APIs e schema das respostas em bruto
│   ├── ARQUITETURA.md             # processo medallion, leituras incrementais, reprocessamento, mapa do notebook
│   ├── TABELAS.md                 # dicionário de dados das tabelas (controlo, bronze, silver, gold) e linhagem
│   ├── METODOLOGIA.md             # hipótese, empresas e setores, regras estatísticas, limitações
│   ├── ANALISES.md                # queries, gráficos, dashboard e modelos de ML
│   ├── CONCLUSOES.md              # resultados e conclusões
│   ├── ESTADO.md                  # checklist e prazos
│   ├── DESENVOLVIMENTO.md         # correr e testar fora do Databricks
│   └── MANUAL_ABD.md              # manual de estudo da cadeira (teoria: lakehouse, Spark, Delta)
├── Project_Instructions.md        # enunciado do professor (aula 7 + regras de entrega e apresentação)
├── CLAUDE.md                      # instruções para o assistente Claude Code (não é preciso para ler o projeto)
├── requirements.txt               # bibliotecas usadas (já incluídas no Databricks)
├── requirements-local.txt         # bibliotecas para correr localmente (PySpark + Jupyter)
├── .gitignore                     # exclui o .venv/ e o lakehouse local (lakehouse_oil_psi/)
└── README.md
```

---

## Fontes de dados

São 21 séries gratuitas e sem chave:
- **FRED:** Brent, WTI, EUR/USD, o índice OCDE de ações portuguesas e o IHPC de Portugal;
- **Yahoo Finance:** o índice PSI e 15 empresas cotadas.

Não existe PSI diário gratuito antes de 2013; por isso usam-se também o índice mensal da OCDE e as cotações das empresas.
Como o Databricks Free Edition restringe o acesso à internet, o notebook usa ficheiros *seed* quando a API falha.
→ Detalhe e schemas das respostas: [docs/FONTES.md](docs/FONTES.md).

## Arquitetura (Medallion)

```
APIs ─► LANDING (ficheiros em bruto) ─► BRONZE (append-only, linhagem) ─► SILVER (normalizada, MERGE) ─► GOLD (analítica, otimizada)
                                                                                                          └─► Spark SQL · matplotlib · AI/BI Dashboard · MLlib
```

Há 3 leituras incrementais (carga histórica + 2 incrementais, com 45 dias de sobreposição). A Silver pode ser reprocessada de raiz a partir
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
| Tabelas | Delta Lake + Unity Catalog (`workspace.oil_psi`) | Parquet em `./spark-warehouse/oil_psi.db/` (ignorada pelo git) |
| Ficheiros (landing, seed) | Volume `/Volumes/workspace/oil_psi/lake` | pasta `./lakehouse_oil_psi/` (ignorada pelo git) |
| Seeds | copiados automaticamente de `dados_fonte/` (Git folder); senão, *upload* manual | copiados automaticamente de `dados_fonte/` |
| Células `%sql` | nativas | convertidas automaticamente em `spark.sql(...)` |
| `display()` | tabelas e gráficos interativos | tabela em texto |
| `MERGE`, `OPTIMIZE … ZORDER`, *time travel* | sim (Delta) | o `MERGE` passa a *anti-join + union*; os outros são saltados |
| Internet | só domínios "confiáveis": as APIs bloqueadas leem o seed | as APIs costumam responder: dados mais recentes, números diferentes de [CONCLUSOES.md](docs/CONCLUSOES.md) |
| Entrega em HTML | sim (*File → Export → HTML*) | não serve para a entrega |

O notebook deteta sozinho o ambiente (`IS_DATABRICKS`). O mesmo ficheiro corre nos dois sem alterações.

### A. Databricks Free Edition (execução oficial)

As bibliotecas usadas (`pyspark`, `requests`, `pandas`, `numpy`, `matplotlib`) já vêm instaladas no Databricks; o [requirements.txt](requirements.txt) serve apenas como documentação.

1. Abrir o repositório como **Git folder** (*Workspace → Create → Git folder*, com o URL do GitHub), ou atualizá-lo com *Pull*.
   Também se pode importar só o notebook (*Workspace → Import*), mas então os seeds não são copiados sozinhos (ver passo 3).
2. Correr as células da secção **0. Configuração do ambiente**. Criam o schema `workspace.oil_psi` e o Volume `lake` e copiam os
   21 seeds de `dados_fonte/` para `/Volumes/workspace/oil_psi/lake/seed/`. A cópia é idempotente: só copia o que falta.
3. *Só se o notebook foi importado sem a Git folder:* em **Catalog → workspace → oil_psi → Volumes → lake**, criar a pasta `seed` e fazer *upload* dos **21 ficheiros** de `dados_fonte/`.
4. Correr **Run all**. Com `RESET = True` todas as tabelas e a landing são apagadas e reconstruídas (o seed é mantido).
5. Criar o **AI/BI Dashboard** (*New → Dashboard*) sobre as tabelas `gd_*` (widgets propostos na secção 6.2 do notebook; especificação em [docs/ANALISES.md](docs/ANALISES.md)).
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
3. Clicar em **Run all**. O notebook encontra o Java, copia os seeds e corre as células `%sql` sozinho.

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
   - copia os seeds de `dados_fonte/` para `./lakehouse_oil_psi/seed/`;
   - corre as células `%sql`.
3. Em alternativa, pela linha de comandos:
   `jupyter nbconvert --to notebook --execute --ExecutePreprocessor.kernel_name=projecto-abd Lakehouse_Petroleo_PSI.ipynb --output run_local.ipynb`

Problemas comuns e detalhes: [docs/DESENVOLVIMENTO.md](docs/DESENVOLVIMENTO.md).

---

## Principais resultados

_Execução local de 07-11-2026 com os ficheiros seed (PySpark 4.3, modo SEED). Os números devem ser confirmados depois da execução no Databricks._

- **Galp — confirma a hipótese.** Num mês em que o Brent (EUR) sobe 10%, a Galp sobe em média +2,7% (t = 7,1), e +2,1% descontando a bolsa (t = 5,6).
- **As outras 14 — não confirmam no efeito total.** Todas têm correlação positiva com o petróleo e o cabaz sem a Galp sobe +1,0% por cada +10% no Brent (t = 4,3): petróleo e bolsa seguem o mesmo ciclo económico.
- **Descontando o movimento da bolsa**, 7 das 14 ficam negativas; a REN e os CTT confirmam a hipótese de forma significativa.
- **A relação depende do tipo de choque:** juntos nas crises de procura (2020–22); no choque de 2025–26 a correlação de 36 meses do cabaz sem a Galp caiu para −0,32, enquanto a Galp ficou em +0,50.
- **Prever o PSI com o petróleo não funciona:** R² de 5% e um Random Forest abaixo da regra ingénua "sobe sempre" (AUC 0,44).

→ Todos os resultados e o mapa das conclusões do notebook: [docs/CONCLUSOES.md](docs/CONCLUSOES.md).

## Estado do projeto

Checklist e prazos (entrega até 16-11-2026, 23:59; apresentação a 17 ou 19-11-2026): [docs/ESTADO.md](docs/ESTADO.md).
