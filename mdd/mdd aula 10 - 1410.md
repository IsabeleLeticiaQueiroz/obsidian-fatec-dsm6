##### Bag of Words
↪︎Técnica em PLN utilizada para transformar bases de dados textuais em vetores numéricos 
↪︎Essa técnica pode ser divida em alguns passos:
**① Criação do vocabuçário:** analisa-se todos os registros do dataset e o processo de tokenização é aplicado. O conjunto de palavras oriundas da tokenizaçãode todos os registros é chamado de ==vocabulário==. Nesse vocabulário não são armazenados termos repetidos.

**② Contagem de frequência:** apesar de não armazenar os termos repetidos no vocabulário a cada ocorrência ==adiciona +1 a posição== correspondente da array.

**➂ Ignorar a ordem:** nessa técnica a estrutura gramatical, ordem que o termo aparece são ignoradas. O seu funcionamento é semelhante a ter uma "urna" e cada novo termo seria uma "bolinha".

Existem variações dessas técnicas, mas a prática tradicional é realizar a seguinte análise:
- se  palavra pertence a frase então preencher com "1" aquela posição da matriz
- se palavra não pertence a frase então preencher com "0" aquela posição da matriz
↪︎Exemplo prático
``documentos = ["Eu gosto de programação", "Eu gosto de Python", "Python é uma linguagem de programação"]
1. Criar vocabulário:
**tokenizar frase 1** = ``["Eu", "gosto", "de", "programação"]
**tokenizar frase 2** = ``["Eu", "gosto", "de", "Python"]
**tokenizar frase 3** = ``["Python", "é", "uma", "linguagem", "de", "programação"]

**vocabulário =** ``["Eu", "gosto", "de", "programação", "Python", "é", "uma", "linguagem"]

2. Contagem da frequência
**freq =** ``[("Eu", 2), ("gosto", 2), ("de", 3), ("programação",2), ("Python", 2), ("é", 1), ("uma", 1), ("linguagem", 1)]

3. Transformando os textos em vetores numéricos

|                                         | Eu  | gosto | de  | programação | Python | é   | uma | linguagem |
| --------------------------------------- | --- | ----- | --- | ----------- | ------ | --- | --- | --------- |
| "Eu gosto de programação"               | 1   | 1     | 1   | 1           | 0      | 0   | 0   | 0         |
| "Eu gosto de Python"                    | 1   | 1     | 1   | 0           | 1      | 0   | 0   | 0         |
| "Python é uma linguagem de programação" | 0   | 0     | 1   | 1           | 1      | 1   | 1   | 1         |

