
##### 1. Tokenização

Usando `nltk.tokenize.word_tokenize()` sobre o corpus (texto convertido para minúsculas e filtrando apenas tokens alfabéticos, isto é, descartando pontuação):

- **Total de tokens:** 4.939
- **Total de tokens diferentes (vocabulário):** 1.664
- **Token mais frequente:** `de` (194 ocorrências)

Isso já era esperado: preposições e artigos costumam dominar o topo do ranking de frequência em qualquer texto em português, o que motiva a próxima etapa.

##### 2. Remoção de stopwords

Aplicando `nltk.corpus.stopwords.words('portuguese')` sobre o conjunto de tokens únicos:

- **Tokens únicos antes da remoção:** 1.664
- **Tokens únicos após a remoção:** 1.554
- **Stopwords removidas:** 110

##### 3. Top 10 palavras mais frequentes após o pré-processamento

|#|Palavra|Ocorrências|
|---|---|---|
|1|mundo|44|
|2|novo|39|
|3|admirável|33|
|4|john|29|
|5|huxley|22|
|6|bernard|21|
|7|sociedade|19|
|8|selvagem|19|
|9|lenina|18|
|10|realidade|17|

---

##### Respostas ao Relatório

##### Questão 1 - Qual corpus foi utilizado?

- **Tema:** Literatura — análises, resumos e resenhas sobre o romance _Admirável Mundo Novo_, de Aldous Huxley (personagens, enredo, contexto histórico e interpretação da obra).
- **Origem:** Compilação de 10 textos (resumos/resenhas/análises) sobre a mesma obra, reunidos em um único arquivo `corpus.txt`.
- **Quantidade aproximada de palavras:** ~5.050 palavras (30.909 caracteres).

##### Questão 2 - Quantos tokens foram encontrados inicialmente?

Foram encontrados **4.939 tokens** (considerando apenas tokens alfabéticos, após tokenização com `word_tokenize` e conversão para minúsculas). O vocabulário (tokens únicos) totalizou **1.664 palavras diferentes**.

##### Questão 3 - Quantos tokens permaneceram após a remoção das stopwords?

Do conjunto de **1.664 tokens únicos**, restaram **1.554 tokens** após a remoção das stopwords — ou seja, **110 stopwords** (como "de", "a", "o", "que", "e", "em", "um", "para", "com", etc.) foram eliminadas do vocabulário.

##### Questão 4 - Quais foram as 10 palavras mais frequentes após o pré-processamento?

1. mundo (44)
2. novo (39)
3. admirável (33)
4. john (29)
5. huxley (22)
6. bernard (21)
7. sociedade (19)
8. selvagem (19)
9. lenina (18)
10. realidade (17)

Nota-se que, após a remoção das stopwords, o ranking passou a ser dominado pelos **termos centrais do conteúdo real do corpus**: o título da obra ("mundo", "novo", "admirável"), nomes de personagens ("john", "bernard", "lenina") e conceitos-chave da narrativa ("sociedade", "selvagem", "realidade"), muito mais informativo do que o ranking inicial, dominado por preposições e artigos.

##### Questão 5 - Por que a remoção de stopwords pode ser importante em determinadas aplicações de PLN?

A remoção de stopwords é importante porque palavras como artigos, preposições e conjunções aparecem com altíssima frequência em qualquer texto, mas carregam pouco ou nenhum significado, servem principalmente para dar coesão à frase, não para indicar do que o texto trata. Em aplicações como análise de frequência de palavras (nuvens de palavras, extração de palavras-chave), e entre outros, manter as stopwords tende a "poluir" os resultados: o sistema passaria a destacar palavras como "de", "que" e "em" como as mais relevantes, quando na verdade elas não ajudam a diferenciar um texto de outro nem a entender seu conteúdo. Ao removê-las, o processamento se concentra nos termos de conteúdo, reduzindo a dimensionalidade do vocabulário, economizando processamento e produzindo resultados muito mais informativos e interpretáveis, como ficou evidente na comparação entre o token mais frequente antes ("de") e as palavras mais frequentes depois da remoção, que realmente refletem o assunto do corpus.

---
##### Código
```Python
import nltk

from nltk.tokenize import word_tokenize

from nltk.corpus import stopwords

from collections import Counter

  

for pacote in ["punkt", "punkt_tab", "stopwords"]:

    try:

        nltk.data.find(f"tokenizers/{pacote}")

    except LookupError:

        try:

            nltk.download(pacote, quiet=True)

        except Exception:

            pass

  
  

with open("corpus.txt", "r", encoding="utf-8") as f:

    texto = f.read()

  

print("=" * 60)

print("1) CORPUS CARREGADO")

print("=" * 60)

print(f"Quantidade aproximada de palavras (split simples): {len(texto.split())}")

print(f"Quantidade de caracteres: {len(texto)}")

  
  

texto_lower = texto.lower()

  

tokens = word_tokenize(texto_lower, language="portuguese")

  

tokens_palavras = [t for t in tokens if t.isalpha()]

  

total_tokens = len(tokens_palavras)

tokens_unicos = set(tokens_palavras)

qtd_tokens_unicos = len(tokens_unicos)

  

freq = Counter(tokens_palavras)

token_mais_frequente, freq_mais_frequente = freq.most_common(1)[0]

  

print("\n" + "=" * 60)

print("2) TOKENIZAÇÃO")

print("=" * 60)

print(f"Total de tokens (palavras): {total_tokens}")

print(f"Total de tokens diferentes (vocabulário): {qtd_tokens_unicos}")

print(f"Token mais frequente: '{token_mais_frequente}' ({freq_mais_frequente} ocorrências)")

  

stopwords_pt = set(stopwords.words("portuguese"))

tokens_sem_stopwords = tokens_unicos - stopwords_pt

  

print("\n" + "=" * 60)

print("3) REMOÇÃO DE STOPWORDS")

print("=" * 60)

print(f"Tokens únicos antes da remoção: {len(tokens_unicos)}")

print(f"Tokens únicos após remoção de stopwords: {len(tokens_sem_stopwords)}")

print(f"Stopwords removidas: {len(tokens_unicos) - len(tokens_sem_stopwords)}")

  
  

freq_sem_stopwords = Counter(

    t for t in tokens_palavras if t in tokens_sem_stopwords

)

  

top10 = freq_sem_stopwords.most_common(10)

  

print("\n" + "=" * 60)

print("4) TOP 10 PALAVRAS MAIS FREQUENTES (sem stopwords)")

print("=" * 60)

for i, (palavra, contagem) in enumerate(top10, start=1):

    print(f"{i:2d}. {palavra:<15} {contagem} ocorrências")
```