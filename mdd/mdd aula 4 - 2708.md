
| Média                | Mediana       | Moda                       |
| -------------------- | ------------- | -------------------------- |
| Descobre valor médio | valor no meio | valor com maior frequencia |

==dataset iris==: dataset muito usado para ML, e se tornou um padrão de algoritmo muito bem sucedido para a área, pois foi estudado nos anos 1939, durante o estudo de uma espécie de planta para identificar a espécie.

---
##### Montagem de algoritmo
- arquivo iris.py, coloquei na mesma pasta da aula passada...
```
# classificar flores de 3 especies

  

# importando bibliotecas

import pandas as pd

import matplotlib.pyplot as plt

  

from sklearn.datasets import load_iris

from sklearn.tree import plot_tree

  

# metricas utilizadas para avaliar o modelo

from sklearn.metrics import accuracy_score

from sklearn.metrics import confusion_matrix

from sklearn.metrics import classification_report

from sklearn.metrics import ConfusionMatrixDisplay

  

# carregando o dataset iris

iris = load_iris()

  

print('\n')

print('=' * 70)

print('Dataset Iris')

print('=' * 70)

  

print("\nDataset carregado com sucesso!")

  

# conhecendo o dataset

print("Quantidade de registros:")

print(len(iris.data))

  

# nome das características

print("\nCaracterísticas:")

for caracteristica in iris.feature_names:

    print("-", caracteristica)

  

# nomes das especies

print("\nEspécies:")

for especie in iris.target_names:

    print("-", especie)

  

# visualizando uma flor

print("\n")

print("=" * 70 )

print("Exemplo de uma flor do dataset")

print("=" * 70 )

print("Medidas da primeira flor do dataset (cm):")

print("Comprimento da sépala:", iris.data[0][0])

print("Largura da sépala:", iris.data[0][1])

print("Comprimento da pétala:", iris.data[0][2])

print("Largura da pétala:", iris.data[0][3])

  

# codigo da especie

codigo_especie = iris.target[0]

print("Código da espécie:", codigo_especie)

  

# traduzindo o codigo da especie para o nome da especie

nome_especie = iris.target_names[codigo_especie]

print("Nome da espécie:", nome_especie)

  

# transformando dados em tabela

print("\n")

print("=" * 70)

print("Transformando dados em tabela")

print("=" * 70)

# criar um dataframe com os dados do dataset iris

dados = pd.DataFrame(data=iris.data, columns=[

    'comprimento_sepala', 'largura_sepala', 'comprimento_petala', 'largura_petala'])

  

# adicionar a coluna 'especie' ao dataframe

dados["codigo especie"] = iris.target

dados["nome especie"] = dados["codigo especie"].map({

    0: 'setosa',

    1: 'versicolor',

    2: 'virginica'

})

  

print(dados.head(10))

  

# tamanho do dataset

print("\n")

print("=" * 70)

print("Tamanho do dataset")

print("=" * 70)

print("Número de linhas:", dados.shape[0])

print("Número de colunas:", dados.shape[1])

  

# quantidade de flores de cada especie

print("\n")

print("=" * 70)

print("Quantidade de flores de cada espécie")

print("=" * 70)

quantidade_flores = dados["nome especie"].value_counts()

print(quantidade_flores)

  

# grafico quantidade de flores de cada especie

print("\n")

print("=" * 70)

print("Gráfico quantidade de flores de cada espécie")

print("=" * 70)

quantidade_flores.plot(kind='bar', color=['blue', 'pink', 'purple'])

plt.title("Quantidade de flores de cada espécie")

plt.xlabel("Espécies")

plt.ylabel("Quantidade")

plt.xticks(rotation=0)

plt.tight_layout()

plt.show()
```