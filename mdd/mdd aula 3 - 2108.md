##### SEMMA
==Origem==
↪︎Desenvolvido pelo SAS INSTITUTE para guiar o trabalho de modelagem em sua ferramenta ENTERPRISE MINER
==Objetivo==
↪︎Foco exclusivo na engenharia de modelagem
==Aplicação==
↪︎Utilização por equipes de estatística e ciência de dados

**S** -> SAMPLE 
**E** -> EXPLORE
**M**-> MODIFY
**M**-> MODEL
**A** -> ASSESS

 **Etapas:**
==Sample (Amostra)==: Extrai um subconjunto representativo permitindo um processamento ágil no computador
==Explore (Explorar)==: Descobrir anomalias, lacunas e relações visuais através de análises univariadas[^1] e multivariadas. 
==Modify (Modificar)==: Transforma, agrupa variáveis. Limpar o dado e preparar para o algoritmo.
==Model (Modelo)==: Aplicar modelos preditivos e técnicas matemáticas sobre variáveis preparadas.
==Assess (Avaliar)==: Testar a viabilidade estatística do modelo criado contra novos subconjuntos de validação.


| DIMENSÃO                   | KDD                    | CRISP-DM                   | SEMMA                            |
| -------------------------- | ---------------------- | -------------------------- | -------------------------------- |
| Origem/Foco                | Acadêmico              | Indústria                  | SAS Institute                    |
| Entendimento do<br>negócio | Implícito no<br>início | Fase Crítica e<br>Dedicada | [Não aborda]<br>Assume-se pronto |
| Tratamento de <br>dados    | 3 fases                |                            |                                  |
|                            |                        |                            |                                  |

---
- Instalação de bibliotecas citadas no grupo do whats
- criar pasta, adicionar .venv, criar arquivo "config_teste.py"
```
import numpy as np

import pandas as pd

import matplotlib.pyplot as plt

import seaborn as sns

import sklearn as sk

  

print("Numpy version:", np.__version__)

print("Pandas version:", pd.__version__)

print("Scikit-learn version:", sk.__version__)
```
- ativar word wrap no settings do vscode
- criar aquivo exemplopandas.py na mesma pasta do outro arquivo
```
import pandas as pd

# criar um dataframe q vai ter uma sinulacao de dados, manualmente mesmo, mas proximas aulas sera arquivo csv

dados = {

    'nome': ['João', 'Maria', 'Pedro', 'Ana', 'Lucas'],

    'idade': [25, 30, 22, 28, 35],

    'salario': [3000, 4000, 2500, 3500, 4500],

    'departamento': ['Vendas', 'Marketing', 'TI', 'Financeiro', 'RH']

}

  

df = pd.DataFrame(dados)

print(df)

  

# existem alguns comandos que podem ser usados para explorar o dataframe, como por exemplo:

#  quando queremos ver as primeiras linhas do dataframe, podemos usar o comando head(), é utilizado apenas pra verificar se esta sendo importado corretamente, etc.

print(df.head(2))

  

# estatistica basica das colunas

print(df.describe())

  

# filtro por condição

print(df[df['idade'] > 30])
```
---
[^1]: A análise univariada examina apenas uma variável por vez para entender sua distribuição e medidas resumo. Já a análise multivariada estuda três ou mais variáveis de forma simultânea para avaliar relações complexas e o efeito combinado entre elas
