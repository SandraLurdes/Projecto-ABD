# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

Este ficheiro é para o agente: regras, onde procurar e como decidir. **O que é o projeto** (grupo, objetivo, hipótese, como executar,
resultados) está no [README.md](README.md). Não o repitas aqui.

## 1. O essencial

- **O entregável é `Lakehouse_Petroleo_PSI.ipynb`** e é a fonte de verdade. Não há código fora dele, nem *build*, *lint* ou testes
  automáticos. Os documentos de `docs/` descrevem o notebook e citam as secções e células.
- **Neste Mac o notebook corre localmente** (desde 10-11-2026: `.venv/` com PySpark 4.3 + Java 17 do Homebrew, kernel `projecto-abd`).
  Verifica uma alteração com `.venv/bin/jupyter nbconvert --to notebook --execute --ExecutePreprocessor.kernel_name=projecto-abd ...`
  (ver [docs/DESENVOLVIMENTO.md](docs/DESENVOLVIMENTO.md)). Isso **não** testa o Databricks (Delta, Volumes, serverless): diz isso ao utilizador.
- **Língua:** pt-PT em todo o texto, nos comentários e nos nomes visíveis ao utilizador.
- **Não inventes conteúdo** (números, colunas, fontes). Se não conseguires confirmar no notebook ou nos ficheiros de fallback, marca com `[FLAG]`.

## 2. Onde procurar

| Para… | Lê |
|---|---|
| Perceber o projeto e como se executa | [README.md](README.md) |
| Saber o que o professor exige (enunciado, entrega, apresentação, divergências) | [Project_Instructions.md](Project_Instructions.md) |
| Mexer nas APIs, no fallback, no formato em bruto, ou acrescentar uma série | [docs/FONTES.md](docs/FONTES.md) |
| Mexer no fluxo entre camadas, nos batches/watermark, no `MERGE`, no reprocessamento ou no `RESET`; localizar uma função no notebook | [docs/ARQUITETURA.md](docs/ARQUITETURA.md) |
| Mexer em colunas ou tabelas, escrever uma query, seguir a origem de uma coluna | [docs/TABELAS.md](docs/TABELAS.md) |
| Mexer em estatísticas, veredictos, setores ou sinais esperados | [docs/METODOLOGIA.md](docs/METODOLOGIA.md) |
| Mexer nas queries, nos gráficos ou no ML, ou criar o AI/BI Dashboard | [docs/ANALISES.md](docs/ANALISES.md) |
| Citar ou rever números e conclusões | [docs/CONCLUSOES.md](docs/CONCLUSOES.md) |
| Ver o que falta fazer e os prazos | [docs/ESTADO.md](docs/ESTADO.md) |
| Correr ou testar fora do Databricks (`requirements-local.txt`, `.gitignore`) | [docs/DESENVOLVIMENTO.md](docs/DESENVOLVIMENTO.md) |
| Rever a teoria da cadeira (lakehouse, Spark, Delta) | [docs/MANUAL_ABD.md](docs/MANUAL_ABD.md) (grande: procura por secção, não o leias todo) |
| Confirmar o formato real de uma fonte | `dados_fallback/` (cópia antiga das APIs, com o formato exato; só usada quando a API falha) |

Para encontrar código no notebook, faz o dump das células com um script Python sobre o JSON e pesquisa por nome de função
(o mapa está em [docs/ARQUITETURA.md](docs/ARQUITETURA.md#7-mapa-do-notebook-funções-por-secção)).

## 3. Requisitos do professor (critério para decidir)

Qualquer alteração tem de manter cumpridos estes requisitos (o detalhe está em [Project_Instructions.md](Project_Instructions.md)):
- Data Lakehouse com Apache Spark no **Databricks Free Edition**.
- Pelo menos uma fonte, lida **mais do que uma vez** (3 leituras incrementais).
- Camadas Bronze (histórico das leituras, reprocessável) → Silver (normalizada, modelada para o tema) → Gold (otimizada para análise).
- Análises: **query, dashboard e modelo de ML**.
- Conclusões sobre os dados, o processo e a tecnologia.
- Entrega num **notebook em HTML** com cabeçalho com o nome do grupo e dos elementos.

Seguimos o estilo do notebook da aula 7 ("ABD07 The Data Lakehouse.html", na pasta-mãe):
- Volumes em `/Volumes/workspace/...` e um schema próprio;
- prefixos `bz_`/`sv_`/`gd_`;
- "data rules" na Bronze/Silver e "business rules" na Silver.

Na dúvida entre duas soluções, escolhe a que fica mais perto deste estilo.

## 4. Regras invioláveis

**Edição do notebook**
- O grupo edita o notebook no Databricks, numa **Git folder ligada a este repositório**. Antes de alterares o `.ipynb`, confirma que o
  repositório local está sincronizado (`git pull`). Só é preciso uma exportação (*File → Export → IPython Notebook*) se alguém editou
  fora da Git folder.
- Só alteras o notebook se o pedido for explicitamente sobre ele. Em trabalho de documentação, é só de leitura.
- Nunca apagues texto ou comentários escritos pelo grupo. Os comentários da ferramenta de comentários do Databricks não vêm no `.ipynb`.
- O `.ipynb` é JSON com metadados do Databricks (`application/vnd.databricks.v1+notebook`). Para editar células usa `NotebookEdit` ou um
  script Python sobre o JSON e preserva esses metadados, para a reimportação no Databricks continuar a funcionar.
- As células SQL começam por `%sql`.
- **Passos de preparação vão para o código, não para instruções manuais**, e têm de ser idempotentes. Exemplos: a cópia do fallback e a
  deteção do Java (secção 0). A documentação de instalação local cobre sempre **macOS e Windows**.
- As funções são definidas na mesma célula em que são usadas pela primeira vez. Por isso o notebook só corre **de cima para baixo**
  (Run all). Uma função nova tem de ficar antes da primeira célula que a usa.

**Compute serverless do Free Edition**
- Não usar `cache()`, `persist()` nem `spark.sparkContext`.
- Usar `_metadata.file_path` em vez de `input_file_name()`.
- Os Volumes estão acessíveis com `open()` / `os` do Python.
- O MLlib é suportado (se falhar, a alternativa é o scikit-learn sobre `toPandas()`).
- A internet só está aberta a uma lista de domínios "confiáveis". Qualquer leitura nova de uma fonte precisa de um ficheiro de fallback
  ([docs/FONTES.md](docs/FONTES.md#4-acesso-à-internet-no-databricks-e-contingência-fallback)).
- O User-Agent é por fonte (FRED sem User-Agent próprio; Yahoo com `YAHOO_HEADERS`). Não os unifiques: o FRED deixa de responder
  (ver [docs/FONTES.md](docs/FONTES.md)).

**Método** (detalhe e justificações em [docs/METODOLOGIA.md](docs/METODOLOGIA.md))
- Correlações, betas e testes **sempre sobre retornos logarítmicos, nunca sobre níveis**.
- Petróleo sempre **em EUR** (`brent_eur = brent_usd / eurusd`). O IHPC só serve para os gráficos em euros de hoje.
- A análise por empresa é **mensal**, com o fecho ajustado. O ML usa uma divisão **temporal** (treino até 2023).

**Visualizações:** só as de matplotlib aparecem no HTML exportado. Os gráficos do `display()` dependem de configuração na interface.

## 5. Manter a documentação sincronizada

Quando mudares alguma coisa no notebook, atualiza o documento do tema:

| Mudança no notebook | Atualizar |
|---|---|
| Nova série, nova API, alteração de `fetch_*` ou do fallback | `docs/FONTES.md` (+ `docs/METODOLOGIA.md` se for uma empresa) |
| Nova tabela ou coluna, mudança de tipo ou de chave | `docs/TABELAS.md` (+ `docs/ARQUITETURA.md` se mudar o fluxo) |
| Batches, watermark, `MERGE`, `RESET`, otimização | `docs/ARQUITETURA.md` |
| Regras estatísticas, veredicto, setores | `docs/METODOLOGIA.md` e rever `docs/CONCLUSOES.md` |
| Queries, gráficos, ML, dashboard | `docs/ANALISES.md` |
| Números (nova execução) | `docs/CONCLUSOES.md` **e** "Principais resultados" no `README.md` (é o único sítio onde os números se repetem de propósito) |
| Passos de execução no Databricks | "Como executar" no `README.md` |
| Novo ficheiro ou pasta na raiz | a árvore do `README.md` e a tabela da secção 2 deste ficheiro |
| Tarefa concluída | `docs/ESTADO.md` |

Mantém a divisão entre os ficheiros:
- o `README.md` é para pessoas (o grupo, o professor, quem abre o repositório);
- este ficheiro é só para o agente;
- o detalhe técnico fica em `docs/`.
