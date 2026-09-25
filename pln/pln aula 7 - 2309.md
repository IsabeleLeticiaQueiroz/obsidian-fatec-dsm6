Aulas anteriores não tiveram conteúdo pois a professora se ausentou.

---
##### Similaridade textual
↪︎Permite identificar o quão próximos dois fragmentos de textos são baseados em sua estrutura (sintaxe) e/ou significado (semântica).
Exemplo:
- Os gatos comem os ratos.
- Os gatos comem os insetos.
↪︎Análise de acordo com as palavras em comum indicaria um alto nível de semelhança entre as duas frases.
↪︎Análise de acordo com o significado das palavras indicaria que as palavras "insetos" e "ratos" não possuem alto índice de similaridade.

↪︎**Tipos de métricas**

``Métrica baseada em termos``

① Medida de Jaccard
↪︎Ela trabalha comparando o conjunto de termos obtidos após a tokenização. A ordem em que os termos aparecem não importa.
Exemplo:
- Os gatos comem os ratos.
- Os gatos comem os insetos.

**Passos a serem realizados:**
1- Definir os 2 fragmentos de texto
2- Aplicar a tokenização sobre os 2 fragmentos de texto e armazenar os resultados em 2 arrays.
```python
token1 = ['Os', 'gatos', 'comem', 'os', 'ratos'];
token2 = ['Os', 'gatos', 'comem', 'os', 'insetos'];
```
3- Aplicar o operador de intersecção entre os termos dos 2 arrays.
token1 ◠ token2 = 4
4- Aplicar o operador de união (◡) entre os termos dos 2 arrays.
token1 ◡ token2 = 6
5- Realizar a divisão entre intersecção por união.

Jaccard = token1 ◠ token2 /  token1 ◡ token2
Jaccard = 4/6
Jaccard ~= 0,67 ou 67% -> isso se chama de ==taxa de similaridade==

##### Exercício de fixação:
A empresa Platz verificou que em seu banco de dados muitos nomes de cidade eram escritos de diferentes maneiras para a mesma cidade. Por exemplo:
↪︎cidade: São Paulo
↪︎variações: SP, Sp, S. Paulo, São Paulo, São P.
Pensando nisso, responda os itens abaixo:
**a)** Compare as variações de 2 em 2 e de acordo com a métrica de Jaccard, calcule a intersecção, a união e o índice de Jaccard.
**b)** Considerando o funcionamento dessa métrica, essa seria uma técnica eficiente para a tarefa proposta?

---
**Respostas**
**a)** 
Primeira dupla de variações:
```python
token1 = ['São', 'Paulo']
token2 = ['SP']
```
token1 ◠ token2 = 0
token1 ◡ token2 = 3
Jaccard = token1 ◠ token2 /  token1 ◡ token2
Jaccard = 0/3
Jaccard = 0
¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨
Segunda dupla de variações:
```python
token1 = ['São', 'Paulo']
token2 = ['Sp']
```
token1 ◠ token2 = 0
token1 ◡ token2 = 3
Jaccard = token1 ◠ token2 /  token1 ◡ token2
Jaccard = 0/3
Jaccard = 0
¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨
Terceira dupla de variações:
```python
token1 = ['São', 'Paulo']
token2 = ['S.', 'Paulo']
```
token1 ◠ token2 = 1
token1 ◡ token2 = 3
Jaccard = token1 ◠ token2 /  token1 ◡ token2
Jaccard = 1/3
Jaccard = 0,333...
¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨
Quarta dupla de variações:
```python
token1 = ['São', 'Paulo']
token2 = ['São', 'Paulo']
```
token1 ◠ token2 = 2
token1 ◡ token2 = 2
Jaccard = token1 ◠ token2 /  token1 ◡ token2
Jaccard = 2/2
Jaccard = 1
¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨¨
Quinta dupla de variações:
```python
token1 = ['São', 'Paulo']
token2 = ['São', 'P.']
```
token1 ◠ token2 = 1
token1 ◡ token2 = 3
Jaccard = token1 ◠ token2 /  token1 ◡ token2
Jaccard = 1/3
Jaccard = 0,333

**b)** Creio que não seria a melhor aplicação, principalmente na questão entre maiúsculos e minúsculos, mesmo que saibamos que "Sp" e "SP" são similares, a aplicação da técnica  de Jaccard não é capaz de provar isso.

---
② Medida de Levenshtein
↪︎Compara sequência de caracteres verificando a menor quantidade de operações válidas para transformar a primeira string na segunda strinf. Essa comparação é realizada por meio de uma matriz, na qual a primeira string será posicionada nas linhas e a segunda nas colunas.
↪︎São operações válidas: 
- inserção
- Remoção
- Substituição
↪︎Exemplo:
Comparar as palavras "dedo" e "dengo"

|     |     | d   | e   | n   | g   | o   |
| --- | --- | --- | --- | --- | --- | --- |
|     | 0   | 1   | 2   | 3   | 4   | 5   |
| d   | 1   | 0   | 1   | 2   | 3   | 4   |
| e   | 2   | 1   | 0   | 1   | 2   | 3   |
| d   | 3   | 1   | 1   | 1   | 2   | 3   |
| o   | 4   | 2   | 2   | 2   | 2   | 2   |
↪︎Para transformar:

| d   | e   | d   | o   |     |
| --- | --- | --- | --- | --- |
| d   | e   | n   | g   | o   |



==Vocabulário== : todas as palavras válidas adquiridas depois da tokenização


> Não exite apenas uma técnica de comparação de palavras, hoje em dia há cerca de 20 tipos, tudo depende de onde e como se aplica.

---

