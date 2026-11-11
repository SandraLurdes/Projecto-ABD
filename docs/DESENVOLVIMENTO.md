# Desenvolvimento e testes fora do Databricks

Este documento explica como correr e testar o notebook localmente.
- A instalação passo a passo (macOS e Windows) e a comparação Databricks vs local estão no [README](../README.md#como-executar).
- As regras para **editar** o `.ipynb` estão no [CLAUDE.md](../CLAUDE.md).
- O comportamento do notebook fora do Databricks (`IS_DATABRICKS`) está em [ARQUITETURA.md](ARQUITETURA.md#6-ambiente-is_databricks).

## Correr localmente

- O notebook corre em PySpark local sem Delta, com caminhos relativos à pasta onde o kernel corre (a do notebook, no VS Code, no
  Jupyter e no `nbconvert`). As duas pastas são geradas e estão no `.gitignore`:
  - os ficheiros (landing e fallback) ficam em `./lakehouse_oil_psi/`;
  - as tabelas ficam em Parquet em `./lakehouse_oil_psi/warehouse/oil_psi.db/` (`spark.sql.warehouse.dir`, definido na célula de
    configuração).
  - O catálogo local é em memória: ao reiniciar o kernel esquece as tabelas, mas as pastas Parquet ficam no disco e o `DROP TABLE`
    não as apaga. Por isso, localmente, o `RESET = True` apaga também a pasta de cada tabela de `ALL_TABLES`. Com `RESET = False`
    num kernel novo, o notebook avisa que a escrita vai falhar (`LOCATION_ALREADY_EXISTS`). Localmente usa-se sempre `RESET = True`
    e corre-se de cima para baixo.
- **Versões testadas:**
  - PySpark 4.3 + Java 17 em Windows (07-11-2026);
  - PySpark 4.3.0 + Java 17 (Homebrew) + Python 3.12 em macOS (10-11-2026).
  - O PySpark 4.0.x tem um erro nos *workers* Python em Windows.
- **O que o notebook faz sozinho** (célula de configuração e célula do schema, secção 0). Tudo é idempotente, por isso repetir o Run all não muda nada:
  - define `PYSPARK_PYTHON`/`PYSPARK_DRIVER_PYTHON` com o Python do kernel;
  - se o `JAVA_HOME` não estiver definido, procura um JDK 17/21:
    - macOS: `/usr/libexec/java_home`, e depois as pastas do Homebrew `/opt/homebrew/opt` e `/usr/local/opt`;
    - Windows: `C:\Program Files\Eclipse Adoptium`, `Java` e `Microsoft`;
  - regista uma *cell magic* `sql` e um *input transformer* que converte a 1.ª linha `%sql` em `%%sql`. As células `%sql` correm com
    `spark.sql(...)` + `display()` sem serem alteradas;
  - cria a `SparkSession` com `spark.driver.memory=2g`, `spark.ui.showConsoleProgress=false` e `spark.log.level=ERROR` (ver
    [Avisos na execução local](#avisos-na-execução-local));
  - copia os ficheiros de fallback de `dados_fallback/` para `./lakehouse_oil_psi/fallback/`. Só copia os que faltam ou têm tamanho diferente;
  - com `RESET = True`, apaga as pastas Parquet das tabelas (ver acima). No Databricks basta o `DROP TABLE`.
- As instruções `OPTIMIZE`, `DESCRIBE HISTORY` e `VERSION AS OF` só correm quando `TABLE_FORMAT == "delta"`.
- O `display()` local imprime `toPandas()` em texto. Só os gráficos matplotlib aparecem como no Databricks.
- Localmente as duas APIs respondem (verificado a 11-11-2026: as 21 séries em modo `API` nas 3 leituras), o que dá dados mais recentes
  do que o fallback. **Não existe uma flag para forçar o modo FALLBACK.** Para reproduzir os números de [CONCLUSOES.md](CONCLUSOES.md),
  faz com que `_http_get` lance uma exceção (por exemplo, trocando os URLs por um domínio inválido numa cópia do notebook).
- Tempo de uma execução local completa: **~46 s** (era ~455 s antes da correção do User-Agent do FRED; ver
  [ARQUITETURA.md](ARQUITETURA.md#desempenho)).

## Avisos na execução local

Medido a 11-11-2026 (macOS, `nbconvert`). Antes das correções, uma execução deixava **436 linhas de avisos**, quase todas nas saídas
das células (e por isso também num HTML exportado localmente). Agora ficam só as linhas de arranque, na célula de configuração. **No
Databricks nada disto aparece nas células:** os avisos do Spark vão para os *logs* do *driver*, e o runtime já traz o pandas 2.x e o
PyArrow. Todas as correções estão no ramo `if not IS_DATABRICKS` ou no `requirements-local.txt`.

| Aviso | Causa | Tratamento |
|---|---|---|
| `WARN WindowExec: No Partition Defined for Window operation!` (~400×) | Janelas globais (`Window.orderBy("date")`) sobre uma única série: são intencionais (ver [ARQUITETURA.md](ARQUITETURA.md#business-rules-silver--gold) e a nota na secção 4 do notebook) | Escondido com `spark.log.level=ERROR` |
| `FutureWarning: PySpark does not yet fully support pandas >= 3.0.0` | pandas 3 instalado | `pandas>=2.2,<3` no `requirements-local.txt` |
| `UserWarning: toPandas attempted Arrow optimization… PyArrow >= 18.0.0 must be installed` | faltava o PyArrow | `pyarrow>=18` no `requirements-local.txt` (o `display()` também fica mais rápido) |
| `WARN MemoryManager: Total allocation exceeds 95%…` + `Scaling row group sizes…` | heap de 1 GB para 8–10 escritas Parquet em paralelo | `spark.driver.memory=2g` |
| Barras `[Stage N:> (0 + 10) / 10]` | barra de progresso da consola do Spark | `spark.ui.showConsoleProgress=false` |
| `DAGScheduler: Broadcasting large task binary`, `SparkStringUtils: Truncated the string representation of a plan`, `Utils: SparkUI could not bind on port 4040`, `Instrumentation: regParam is zero` | planos grandes (muitas colunas de janela), outro Spark aberto (ex.: o kernel do editor), regressão logística sem regularização | inofensivos; escondidos com `spark.log.level=ERROR` (mudar o `regParam` mudaria os resultados do ML) |
| `WARNING: Using incubator modules: jdk.incubator.vector`, `NativeCodeLoader…`, `Setting default log level…` | escritos pela JVM/Spark ao arrancar, antes de o nível de log se aplicar | uma vez por kernel; ficam |

Com a alteração, os números ficam iguais. A única diferença visível é que, no `display()` local, alguns nulos aparecem como `None` em
vez de `NaN` (efeito do Arrow).

## Instalação

Os passos (macOS e Windows) estão no [README](../README.md#b-local-só-para-testes). Para correr sem abrir o editor:

```bash
.venv/bin/jupyter nbconvert --to notebook --execute --ExecutePreprocessor.kernel_name=projecto-abd Lakehouse_Petroleo_PSI.ipynb --output run_local.ipynb
```

No Windows: `.venv\Scripts\jupyter ...`. No fim pode aparecer um `EOFError` do PySpark ao fechar o kernel; não indica nenhum erro nas células.

## Problemas comuns

| Erro | Causa | Solução |
|---|---|---|
| `ModuleNotFoundError: No module named 'pyspark'` | kernel errado (ex.: "Python 3 (ipykernel)" do Anaconda/sistema) | escolher o kernel **"Projecto ABD (PySpark local)"** |
| O kernel "Projecto ABD (PySpark local)" não aparece no VS Code / Cursor | lista de kernels em cache, ou a pasta aberta no editor não é `Projecto-ABD` (o `.venv` não é detetado) | *Developer: Reload Window*; depois *Select Kernel → Jupyter Kernel…* (botão de atualizar); ou *Python Environments… → Enter interpreter path…* com `.venv/bin/python` (Windows: `.venv\Scripts\python.exe`); ou abrir a pasta `Projecto-ABD` |
| `Unable to locate a Java Runtime` / `JAVA_GATEWAY_EXITED` | não há Java 17/21, ou está numa pasta que o notebook não conhece | instalar o JDK 17 ou definir o `JAVA_HOME` antes de abrir o editor |
| `UnsatisfiedLinkError ... NativeIO$Windows` / `HADOOP_HOME unset` (Windows) | falta o `winutils.exe` | instalar o `winutils.exe` + `hadoop.dll` e definir o `HADOOP_HOME`; sem ele, testa-se com as tabelas em vistas temporárias |
| `Python worker failed to connect back` (Windows) | PySpark 4.0.x, ou os *workers* a usar outro Python | usar o PySpark ≥ 4.1 (o notebook já fixa o `PYSPARK_PYTHON`) |
| `[LOCATION_ALREADY_EXISTS] Cannot name the managed table as ...` | pastas Parquet de um kernel anterior que o catálogo em memória já não conhece (`RESET = False`, ou uma versão antiga do notebook que gravava em `./spark-warehouse/`) | pôr `RESET = True` e correr de novo (Run all); a pasta antiga `./spark-warehouse/` pode ser apagada |
| Muitos `FutureWarning` do pandas 3 ou `UserWarning` "PyArrow … must be installed" nas células | `.venv` criado antes de 11-11-2026 (pandas 3, sem PyArrow) | reinstalar: macOS `.venv/bin/pip install -r requirements-local.txt`, Windows `.venv\Scripts\pip install -r requirements-local.txt` (com `uv`: `uv pip install --python .venv -r requirements-local.txt`) e reiniciar o kernel |
| `UsageError: Line magic function %sql not found` | a célula de configuração (secção 0) não correu nesta sessão | correr o notebook de cima para baixo (Run all) |

## Bibliotecas

- **Databricks:** `pyspark`, `requests`, `pandas`, `numpy` e `matplotlib` já vêm instalados. O `requirements.txt` serve só de documentação.
- **Local:** o `requirements-local.txt` junta o `pyspark`, o `ipykernel` e o `jupyter`.
