# Desenvolvimento e testes fora do Databricks

Este documento explica como correr e testar o notebook localmente.
- A instalação passo a passo (macOS e Windows) e a comparação Databricks vs local estão no [README](../README.md#como-executar).
- As regras para **editar** o `.ipynb` estão no [CLAUDE.md](../CLAUDE.md).
- O comportamento do notebook fora do Databricks (`IS_DATABRICKS`) está em [ARQUITETURA.md](ARQUITETURA.md#6-ambiente-is_databricks).

## Correr localmente

- O notebook corre em PySpark local sem Delta, com caminhos relativos à pasta onde o kernel corre (a do notebook, no VS Code, no
  Jupyter e no `nbconvert`). As duas pastas são geradas e estão no `.gitignore`:
  - os ficheiros (landing e seed) ficam em `./lakehouse_oil_psi/`;
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
  - copia os seeds de `dados_fonte/` para `./lakehouse_oil_psi/seed/`. Só copia os que faltam ou têm tamanho diferente;
  - com `RESET = True`, apaga as pastas Parquet das tabelas (ver acima). No Databricks basta o `DROP TABLE`.
- As instruções `OPTIMIZE`, `DESCRIBE HISTORY` e `VERSION AS OF` só correm quando `TABLE_FORMAT == "delta"`.
- O `display()` local imprime `toPandas()` em texto. Só os gráficos matplotlib aparecem como no Databricks.
- Localmente a API costuma responder, o que dá dados mais recentes do que os seeds. **Não existe uma flag para forçar o modo SEED.** Para
  reproduzir os números de [CONCLUSOES.md](CONCLUSOES.md), faz com que `_http_get` lance uma exceção.

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
| `UsageError: Line magic function %sql not found` | a célula de configuração (secção 0) não correu nesta sessão | correr o notebook de cima para baixo (Run all) |

## Bibliotecas

- **Databricks:** `pyspark`, `requests`, `pandas`, `numpy` e `matplotlib` já vêm instalados. O `requirements.txt` serve só de documentação.
- **Local:** o `requirements-local.txt` junta o `pyspark`, o `ipykernel` e o `jupyter`.
