# Fontes de dados

Este documento trata só do que vem **de fora** do lakehouse: as APIs, as séries, o formato em bruto das
respostas e os ficheiros de contingência (*fallback*). As tabelas criadas a partir destes dados estão em
[TABELAS.md](TABELAS.md). O fluxo entre camadas está em [ARQUITETURA.md](ARQUITETURA.md).

Fonte de verdade: notebook, células 2 (tabela de fontes), 7 (`FRED_SERIES`, `YAHOO_SYMBOLS`) e 9 (`fetch_*`, `find_fallback`) e 10 (`ingest`).

## 1. Catálogo das séries (21, todas gratuitas e sem chave)

| Fonte | Série / símbolo | Conteúdo | Unidade | Freq. | Desde |
|---|---|---|---|---|---|
| FRED | `DCOILBRENTEU` | Brent spot | USD/barril | D | 1987 |
| FRED | `DCOILWTICO` | WTI spot (inclui −37,63 a 20-04-2020, valor real) | USD/barril | D | 1986 |
| FRED | `DEXUSEU` | Câmbio: USD por 1 EUR | USD por EUR | D | 1999 |
| FRED / OCDE | `SPASTT01PTM661N` | Índice de preços de ações Portugal | Índice 2015=100 | M | 1988 |
| FRED / Eurostat | `CP0000PTM086NEST` | IHPC Portugal (inflação), usado para preços em euros de hoje | Índice | M | 1996 |
| Yahoo Finance (chart API) | `PSI20.LS` | Índice PSI (OHLCV) | Pontos | D | **2013-04-30** |
| Yahoo Finance | 15 empresas `.LS` (ver abaixo) | Cotações OHLCV + fecho ajustado | EUR | D | 2000 ou IPO |

Cobertura dos ficheiros de fallback do Yahoo (primeiro → último dia, nº de dias):

| Símbolo | Empresa | Desde | Dias |
|---|---|---|---|
| `GALP.LS` | Galp Energia | 2006-10-24 | 5130 |
| `EDP.LS` | EDP | 2000-01-03 | 6906 |
| `EDPR.LS` | EDP Renováveis | 2008-06-04 | 4719 |
| `RENE.LS` | REN | 2007-07-10 | 4949 |
| `NVG.LS` | Navigator | 2000-01-03 | 6906 |
| `ALTR.LS` | Altri | 2000-01-03 | 6906 |
| `SEM.LS` | Semapa | 2000-01-03 | 6906 |
| `COR.LS` | Corticeira Amorim | 2000-01-03 | 6906 |
| `JMT.LS` | Jerónimo Martins | 2000-01-03 | 6906 |
| `SON.LS` | Sonae | 2000-01-03 | 6906 |
| `IBS.LS` | Ibersol | 2000-01-03 | 6906 |
| `NOS.LS` | NOS | 2000-01-03 | 6906 |
| `CTT.LS` | CTT | 2013-12-05 | 3306 |
| `EGL.LS` | Mota-Engil | 2001-01-25 | 6628 |
| `BCP.LS` | BCP | 2000-01-03 | 6906 |
| `PSI20.LS` | PSI (índice) | 2013-04-30 | 3462 |

Todos terminam em 2026-11-06. O setor e o sinal esperado de cada empresa estão em [METODOLOGIA.md](METODOLOGIA.md).

Fontes consideradas e descartadas:
- **Investing.com:** não tem API e proíbe a recolha automática.
- **Euronext:** só dá 2 anos de histórico gratuito.
- **Stooq:** bloqueia os pedidos.

**Não existe PSI diário gratuito antes de 2013.** Por isso usam-se também o índice mensal da OCDE e as cotações das empresas.

## 2. FRED (Federal Reserve Bank of St. Louis)

**Pedido** (`fetch_fred`, uma série por pedido):

```
GET https://fred.stlouisfed.org/graph/fredgraph.csv?id=<ID>&cosd=<AAAA-MM-DD>&coed=<AAAA-MM-DD>
```

- Pede-se uma série por pedido. Num pedido com várias séries de frequências diferentes, o FRED agrega todas para a frequência mais baixa.
- Validação da resposta: o texto tem de começar por `observation_date`. Se não começar, considera-se falha e usa-se o fallback.
- **Sem User-Agent próprio** (`_http_get` sem `headers`, fica o do `requests`). O FRED **não responde** a User-Agents tipo "browser",
  incluindo o do Yahoo: o pedido fica pendurado até ao `timeout` de 30 s (testado a 11-11-2026; com o do `requests` responde em 0,1 s).
  Até essa data, todas as leituras do FRED caíam no fallback por causa disto.

**Schema em bruto (CSV com cabeçalho):**

| Coluna | Tipo em bruto | Significado |
|---|---|---|
| `observation_date` | texto `AAAA-MM-DD` | Data da observação. Nas séries mensais é o dia 1 do mês. |
| `<ID da série>` (ex.: `DCOILBRENTEU`) | texto | Valor. **Os valores em falta vêm como string vazia** (no fallback: 500 no Brent, 649 no WTI, 273 no EUR/USD) ou como `.`, a convenção antiga do FRED. |

Exemplo (`fred_DCOILBRENTEU.csv`):
```
observation_date,DCOILBRENTEU
2000-01-04,23.95
2000-01-05,23.72
```

O nome da 2.ª coluna muda com a série. A Bronze lê-a com `F.col(series_id)` e guarda-a como `value_raw`.

## 3. Yahoo Finance (chart API v8)

**Pedido** (`fetch_yahoo`):

```
GET https://query1.finance.yahoo.com/v8/finance/chart/<SÍMBOLO>?period1=<epoch>&period2=<epoch>&interval=1d
```

- Precisa de `User-Agent` tipo "browser": o notebook envia `Mozilla/5.0 (EDSA ABD student project)` (`YAHOO_HEADERS`). Com o
  User-Agent do `requests` responde HTTP 429. **Não usar o mesmo User-Agent nas duas fontes** (ver FRED acima).
- `period1` corresponde ao início da janela em UTC (`_to_epoch`) e `period2` ao fim da janela + 86400 s, para incluir o último dia.
- Validação da resposta: tem de existir `chart.result[0].timestamp`.

**Schema em bruto (JSON aninhado; só se listam os campos usados ou relevantes):**

```
chart
├── error                                   null quando o pedido corre bem
└── result[0]
    ├── meta
    │   ├── symbol                (str)     ex.: "GALP.LS"           → Bronze.symbol
    │   ├── currency              (str)     "EUR"                    → Bronze.currency
    │   ├── exchangeTimezoneName  (str)     "Europe/Lisbon"          → Bronze.exchange_tz
    │   ├── instrumentType        (str)     "EQUITY" | "INDEX"
    │   ├── firstTradeDate        (int)     epoch do 1.º dia de negociação
    │   ├── longName / shortName  (str)
    │   └── …                               (preço atual, máximos de 52 semanas, períodos de negociação: não usados)
    ├── timestamp[]               (int)     epoch UTC de cada dia (08:00 UTC)      → Bronze.ts_epoch
    └── indicators
        ├── quote[0]
        │   ├── open[]   (float|null)                                     → open_raw
        │   ├── high[]   (float|null)                                     → high_raw
        │   ├── low[]    (float|null)                                     → low_raw
        │   ├── close[]  (float|null)  NÃO ajustado a dividendos          → close_raw
        │   └── volume[] (int|null)                                       → volume_raw
        └── adjclose[0]
            └── adjclose[] (float)     ajustado a dividendos e operações de capital → adjclose_raw
```

- Os arrays de `quote` e `adjclose` estão alinhados por posição com `timestamp[]`. A Bronze desaninha-os com `posexplode` (ver [TABELAS.md](TABELAS.md#bz_yahoo_chart)).
- O `close` não está ajustado. Os retornos das empresas usam o `adjclose`. Se a API não devolver o bloco `adjclose`, a coluna fica nula e a Silver usa o `close`.
- O PSI tem 15 dias com `close = null` no fallback. Estes dias são eliminados na Silver.

## 4. Acesso à internet no Databricks e contingência (fallback)

O **Databricks Free Edition só deixa aceder à internet a uma lista de domínios "confiáveis"**. Cada `fetch_*` funciona assim:
1. tenta a API;
2. se der erro, lê o ficheiro de fallback `fred_<ID>.csv` / `yahoo_<SÍMBOLO>.json` e **filtra-o pela mesma janela de datas**,
   mantendo o formato exato da resposta da API.

O modo fica registado em `ctl_ingestion_log.extract_mode` como `API` ou `FALLBACK (<tipo de erro>)`.

Os 21 pedidos de cada leitura correm em paralelo (`ingest`, 8 de cada vez). Cada série tem o seu próprio fallback; se o Yahoo
limitar os pedidos (HTTP 429), essa série cai no fallback e o log mostra-o.

- **Fallback no repositório:** `dados_fallback/`, 21 ficheiros (5 `fred_<série>.csv` + 16 `yahoo_<símbolo>.json`), cópia de 07-11-2026.
  Não são a fonte principal: só se usam quando a API falha, e não têm dados posteriores a essa data.
- **No Databricks:** os ficheiros vão para `/Volumes/workspace/oil_psi/lake/fallback/`. São copiados automaticamente de `dados_fallback/` pela célula do schema (secção 0) quando o repositório está numa Git folder; senão, faz-se o *upload* manual. A função `find_fallback` também os encontra noutra pasta do Volume (ignora maiúsculas e exclui a `landing/`).
- **Localmente:** os ficheiros são copiados automaticamente para `./lakehouse_oil_psi/fallback/` (ver [DESENVOLVIMENTO.md](DESENVOLVIMENTO.md)).

## 5. Acrescentar uma série

1. Juntá-la a `FRED_SERIES` ou a `YAHOO_SYMBOLS` (notebook, célula 7). Nas empresas, usar `_eq(nome, sector, hypothesis, sector_view)`.
2. Pôr o ficheiro de fallback correspondente em `dados_fallback/` (com o mesmo formato da API).
3. Atualizar a tabela de fontes no markdown do notebook (célula 2), e a tabela de empresas (célula 3) se for o caso.
4. Atualizar este documento e [METODOLOGIA.md](METODOLOGIA.md) (no caso das empresas).
