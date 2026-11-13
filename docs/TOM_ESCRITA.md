# Tom e escrita do notebook

Regras para todo o texto do notebook: células markdown, comentários de código (`#`, `--`) e mensagens `print`.
**Todo o texto novo ou alterado segue este documento** (ver a [regra obrigatória](#10-regra-obrigatória) no fim).

## 1. Para quê

O notebook é o suporte da apresentação: o grupo lê-o em voz alta, célula a célula, em 15 minutos
([Project_Instructions.md](../Project_Instructions.md#presentation)). Depois é entregue em HTML ao professor.

Somos quatro estudantes a apresentar um projeto de Big Data. O texto diz o que fizemos e o que os dados mostram.
Não explica a teoria da cadeira nem tenta convencer.

As marcas de escrita de IA da secção 5 vêm da página da Wikipedia
[Signs of AI writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing), adaptadas ao português.

## 2. Tom

- **Assertivo.** Afirma o resultado com o número: "A correlação mensal é 0,25." Não escreve "parece sugerir" nem
  "poderá eventualmente indicar". A incerteza diz-se uma vez, com o motivo e o valor ("não significativo: o intervalo cruza o zero").
- **Objetivo.** Sem adjetivos de valor ("impressionante", "muito violentos", "excelente"). O número faz o trabalho do adjetivo:
  "o Brent caiu 70%" em vez de "uma queda dramática".
- **Voz do grupo.** Primeira pessoa do plural ("medimos", "usamos") ou impessoal ("a Silver guarda"). Nunca "eu".
  Não falamos com o leitor como um assistente ("vamos agora ver", "repare que").
- **Sem vender.** Não elogiamos o projeto nem a tecnologia. As limitações dizem-se de forma seca, no sítio onde importam.

## 3. Frases

- Uma ideia por frase. Alvo: até 20 palavras. Acima de 30, partir em duas.
- Ordem direta: sujeito, verbo, complemento. O resultado vem primeiro, o pormenor depois.
- Verbos simples: "é", "tem", "lê", "guarda", "junta", "calcula". Não "constitui", "procede à leitura", "desempenha o papel de".
- Voz ativa quando o agente importa: "a Silver elimina os duplicados", não "os duplicados são eliminados pela Silver".
- Parágrafos de 1 a 3 frases.
- Dizer cada coisa uma vez. Não repetir a mesma ideia com outras palavras na frase seguinte.
- Um termo para cada coisa, sempre o mesmo. Se é "Brent", não passa a "crude" e a "barril" no parágrafo seguinte.

## 4. Português de Portugal

- pt-PT com o Acordo Ortográfico de 1990, como no resto do notebook: "ficheiro", "registo", "ecrã", "utilizador", "equipa".
- Números com vírgula decimal (0,25) e espaço nos milhares (12 345). Datas no texto em DD-MM-AAAA; nos gráficos em ISO
  (ver [IBCS_GRAFICOS.md](IBCS_GRAFICOS.md)).
- Termos técnicos em inglês em itálico na primeira vez (*batch*, *watermark*, *forward-fill*). Nomes de código, tabelas e colunas
  entre acentos graves (`gd_market_daily`).
- Nomes próprios com maiúscula (Galp, Brent, COVID-19); nomes comuns sem ela ("crise financeira", "saúde", "defesa").

## 5. Marcas de escrita de IA a evitar

| Marca | Exemplo a evitar | Escrever antes |
|---|---|---|
| Vocabulário de IA | crucial, fundamental, essencial, robusto, abrangente, panorama, paradigma, sinergia, mergulhar, desvendar, potenciar, alavancar, vibrante, notável, inegável, intrincado, multifacetado, "testemunho de" | a palavra simples, ou nada |
| Verbos de enchimento | "destaca", "realça", "sublinha", "evidencia" a importância de… | dizer o facto |
| Evitar o verbo "ser" | "serve como", "constitui", "apresenta-se como", "desempenha um papel" | "é", "tem" |
| Paralelismos de efeito | "não só X, mas também Y"; "não se trata de X, mas de Y"; "mais do que X, é Y" | duas frases: "X. Também Y." |
| Regra de três | três adjetivos ou três itens só para soar completo ("rápido, fiável e escalável") | só os itens que os dados suportam |
| Gerúndio no fim | "…, refletindo / reforçando / demonstrando a relevância do petróleo." | cortar, ou uma frase nova com o facto |
| Inflação de importância | "um marco", "um momento decisivo", "num mundo cada vez mais…", "no atual contexto" | a data e o número |
| Muletas de transição | "É importante notar que", "Vale a pena referir", "De facto", "Além disso" em todas as frases | começar pelo facto |
| Resumos de fecho | "Em suma", "Em resumo", "Em conclusão" | o "**Fim da secção N.**" ou a "**Resposta:**" já fazem isso |
| Atribuição vaga | "os especialistas consideram", "estudos mostram" | a fonte com nome, ou nada |
| Sinalização vazia | "Cada gráfico tem a sua nota de leitura.", "Vejamos agora…" | cortar; a nota aparece a seguir |
| Restos de chatbot | "Certamente", "Aqui está", "Espero que ajude", perguntas retóricas ao leitor | cortar |
| Secções genéricas | "Desafios e perspetivas futuras" com frases vagas e um fecho otimista | limitações concretas, cada uma com o efeito |
| Variação de sinónimos | Brent / crude / ouro negro / barril na mesma secção | um só termo |

## 6. Formatação

- **Negrito** só para o termo-chave ou o número que responde à pergunta. No máximo 2 por frase ou item de lista,
  sem contar o rótulo inicial (`**Mercado:**`).
- Listas com rótulo a negrito só nos blocos já estabelecidos: *Como ler* e **Conclusões**. Um raciocínio escreve-se em frases.
- Listas só para itens paralelos (tabelas, regras, séries).
- Títulos: maiúscula só na primeira palavra e nos nomes próprios. As camadas LANDING, BRONZE, SILVER e GOLD mantêm as maiúsculas
  nos títulos das secções, por convenção do projeto.
- Sem travessão (—) no texto nem nos títulos: usar dois pontos, vírgula ou parênteses. O traço curto (–) fica para intervalos
  (2000–2026) e o sinal menos (−) para valores.
- Sem emoji. Aspas retas ("…").

## 7. Padrões do notebook

Estes padrões já existem e mantêm-se:

- **Entrada da secção:** 1 a 3 frases com o que a secção faz e porquê. Depois o código.
- **Fecho da secção:** "**Fim da secção N.**" e o que ficou feito, com números.
- **Análise (secção 6):** **Pergunta N** → resultado (tabela ou gráfico) → *Como ler* → **Conclusões** → **Resposta:**.
  A resposta começa por "Sim.", "Não." ou "Em parte." e dá o número que a sustenta.
- **Comentários de código:** dizem o porquê, não repetem o código. Uma linha sempre que possível.
- **Mensagens `print`:** curtas, com o número ("Bronze: 12 345 linhas"). Um aviso diz o problema e o que fazer, numa frase.
- **Notas internas do grupo** ("SIMPLIFICAR", "MUDAR TEXTO…", texto entre `[ ]`) ficam como estão até o grupo as resolver.

## 8. Antes e depois

Frases reais do notebook (13-11-2026), reescritas segundo este guia.

| Célula | Antes | Depois | Regra |
|---|---|---|---|
| 9 | "…grava a resposta **sem qualquer alteração** num ficheiro novo. […] Assim cada leitura fica guardada para sempre e pode ser reprocessada." | "Cada leitura grava a resposta da API, tal como chega, num ficheiro novo. A Bronze pode ser reconstruída a partir destes ficheiros." | repetição (o parágrafo anterior já diz "sem os alterar") |
| 17 | "## 2. Camada BRONZE — histórico de leituras (pronto para reprocessamento)" | "## 2. Camada BRONZE: histórico de leituras" | travessão; título curto |
| 65 | "O segundo mostra o valor acumulado desde a entrada da Galp em bolsa. Cada gráfico tem a sua nota de leitura." | "O segundo mostra o valor acumulado desde a entrada da Galp em bolsa." | sinalização vazia |
| 80 | "**1. Petróleo: 26 anos de ciclos muito violentos.**" | "**1. Petróleo: de 9 a 144 USD em 26 anos.**" | adjetivo de valor → número |
| 0 | "quando o preço do petróleo sobe de forma consolidada, a cotação das empresas portuguesas desce — **exceto a Galp**." | "quando o petróleo sobe, as ações portuguesas descem. A exceção é a **Galp**." | travessão; duas frases |
| 4 | "Para quase todas as empresas o petróleo é um **custo** (…); para a Galp é **receita**." na mesma linha que "**exceto a da Galp**" | manter o negrito só em "custo" e "receita" | máximo 2 negritos |

## 9. Checklist de revisão

Antes de dar um texto por feito, confirmar:

- [ ] Cada frase tem uma ideia e até 30 palavras.
- [ ] Nenhuma palavra ou construção da [secção 5](#5-marcas-de-escrita-de-ia-a-evitar).
- [ ] Nenhum travessão (—).
- [ ] No máximo 2 negritos por frase ou item, sem contar o rótulo.
- [ ] Cada conclusão tem um número. Cada "**Resposta:**" começa por "Sim.", "Não." ou "Em parte.".
- [ ] O mesmo termo para a mesma coisa em toda a secção.
- [ ] pt-PT, vírgula decimal, datas DD-MM-AAAA no texto.
- [ ] Os números são os de [CONCLUSOES.md](CONCLUSOES.md).
- [ ] As notas internas do grupo estão intactas.

O script abaixo é só de leitura. Lista, por célula, as palavras da secção 5, os travessões, as linhas com mais de 2 negritos e as
frases com mais de 30 palavras. Nos comentários e `print` das células de código também. Corre a partir da raiz do repositório:

```python
import json, re
PROIBIDAS = ["crucial", "fundamental", "essencial", "robust", "abrangente", "panorama", "paradigma", "sinergia",
             "mergulh", "desvend", "potenci", "alavanc", "destaca", "realça", "sublinha", "vibrante", "notável",
             "inegável", "testemunho", "intrincad", "multifacetad", "serve como", "constitui", "desempenha",
             "não só", "mas também", "é importante", "vale a pena", "em suma", "em resumo", "em conclusão",
             "de facto", "um marco", "cada vez mais", "evidenciando", "reforçando", "refletindo", "demonstrando"]
nb = json.load(open("Lakehouse_Petroleo_PSI.ipynb", encoding="utf-8"))
for i, c in enumerate(nb["cells"]):
    src = "".join(c["source"])
    if c["cell_type"] == "code":   # só comentários e prints
        src = "\n".join(l.strip() for l in src.splitlines()
                        if l.strip().startswith(("#", "--", "print(")) and not l.strip().startswith("%"))
    texto = "\n".join(l for l in src.splitlines() if not l.lstrip().startswith(("|", "<a ")))
    avisos = [f"'{p}'" for p in PROIBIDAS if p in texto.lower()]
    if "—" in texto:
        avisos.append(f"travessão x{texto.count(chr(0x2014))}")
    for linha in texto.splitlines():
        if linha.lstrip().startswith("#") and c["cell_type"] == "markdown":
            continue                                   # títulos
        corpo = re.sub(r"^\s*(- )?\*\*[^*]+:\*\*", "", linha)   # o rótulo "**X:**" no início não conta
        if corpo.count("**") // 2 > 2:
            avisos.append(f"{corpo.count('**') // 2} negritos numa linha: {corpo.strip()[:50]}…")
        for f in re.split(r"(?<=[.!?;])\s+", linha.strip()):
            if len(f.split()) > 30:
                avisos.append(f"frase longa ({len(f.split())} palavras): {f[:50]}…")
    if avisos:
        print(f"célula {i} ({c['cell_type']}):", *avisos, sep="\n  ")
```

As tabelas markdown ficam de fora (o travessão nos títulos das séries vem da fonte). O script não deteta o tom: a checklist continua
a ser lida à mão.

**Primeira execução (13-11-2026):** nenhuma palavra da secção 5; travessões em 10 células e frases longas sobretudo nas entradas
das secções 5 e 6 e nas conclusões. **Depois da revisão (13-11-2026):** só fica a linha das barras A, B e C da célula 62, que são rótulos
e não ênfase (falso positivo aceite).

## 10. Regra obrigatória

**Todo o texto novo ou alterado do notebook (markdown, comentários e `print`) segue este documento** e passa a
[checklist](#9-checklist-de-revisão). Esta regra está também no [CLAUDE.md](../CLAUDE.md) (secções 2, 4 e 5).
