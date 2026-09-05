##### Continuação da análise de desempenho de estudantes
- desempenhoAluno.py
```python
import pandas as pd

import matplotlib.pyplot as plt

from sklearn.model_selection import train_test_split

from sklearn.tree import DecisionTreeClassifier, plot_tree

from sklearn.metrics import accuracy_score, confusion_matrix

#1. Carregar e conhecer a base

df = pd.read_csv("desempenho_estudantes.csv")

print('\nPrimeiros registro: ')

print(df.head(5))

print('\nTamanho da base: ', df.shape)

print('Valores ausentes:')

print(df.isnull().sum())

#2. Selecionar as variaveis uteis para a mineracao

variaveis = ["horas_estudo_semana", "frequencia_percentual", "atividades_entregues", "media_exercicios", "participacao_aulas", "acessos_plataforma_semana", "faltas_mes"]

x = df[variaveis].copy()

y = df["situacao_final"]

# Entradas(X) => Hora, Frequencia, Atividades, Media, Participacao, Acesso e Faltas

# Algoritmo => Arvore de decisao

# Respostas(Y) => Desempenho_satisfatorio ou Precisa_atencao

#3. Separar os dados para treinamento e o teste

X_treino, X_teste, y_treino, y_teste = train_test_split(x, y, test_size=0.25, random_state=42, stratify=y)

#180 Registros

# 25% para teste = 45 registros

# 75% para aprendizagem = 135 registros

# Modelo: Aprender padroes

# Treinamento = Aprender ocm exemplos anteriores

# Teste = verificar se o aprendizado funciona em exemplos reservados

#4. Pre-processar usando apenas informacoes do treinamento

medianas = X_treino.median()

X_treino = X_treino.fillna(medianas)

X_teste = X_teste.fillna(medianas)

#5. Criar e treinar a Árvore de decisão

modelo = DecisionTreeClassifier(max_depth=3, min_samples_leaf=5, random_state=42)

modelo.fit(X_treino, y_treino)

#6. Fazer previsões e avaliar o modelo

previsoes = modelo.predict(X_teste)

acuracia = accuracy_score(y_teste, previsoes)

matriz = confusion_matrix(

    y_teste, previsoes,

    labels = ["Precisa de Atenção", "Desempenho_satisfatorio"]

)

print(f"\nAcurácia do modelo: {acuracia:.2f}")

#7 Descobrir quais variaveis mais influenciaram as decisoes

importancias = pd.Series(modelo.feature_importances_, index=variaveis)

print("\nImportancia das variaveis:")

print(importancias.sort_values(ascending=False).round(3))

  

# 8. Visualizar o padrão aprendido

plt.figure(figsize=(18, 8))

plot_tree(modelo, feature_names=variaveis, class_names=modelo.classes_, filled=True, rounded=True, fontsize=9)

plt.title("Árvore de Decisão - Desempenho dos Estudantes")

plt.tight_layout()

plt.show()

  

# 9. Usar o modelo em um novo caso

novo_aluno: pd.DataFrame = pd.DataFrame([{

    "horas_estudo_semana": 2.0,

    "frequencia_percentual": 65.0,

    "atividades_entregues": 10,

    "media_exercicios": 8.0,

    "participacao_aulas": 2,

    "acessos_plataforma_semana": 3,

    "faltas_mes": 6

}])

previsao_novo_aluno = modelo.predict(novo_aluno)

print(f"\nPrevisão para o novo aluno: {previsao_novo_aluno[0]}")
```

![[arvore-decisao.png]]

==O que traria a informação do desempenho do aluno seria a combinação de parâmetros como:==
media_exercicios             `0.516`
atividades_entregues         0.244
horas_estudo_semana          0.157
acessos_plataforma_semana    0.070

==porém, a média de exercícios pesa mais que o restante.==

---
###### Explicação IA
O algoritmo **não sabe previamente que, por exemplo, nota abaixo de 6 é baixa**. Você não ensinou nenhuma regra do tipo `if media < 6`.

Ele **aprende essas regras olhando para os exemplos que estão no CSV**.

No seu arquivo, existe uma coluna:

```python
situacao_final
```

com as respostas:

- `Desempenho_satisfatorio`
    
- `Precisa_atencao`
    

E existem as características do aluno:

```python
horas_estudo_semana
frequencia_percentual
atividades_entregues
media_exercicios
participacao_aulas
acessos_plataforma_semana
faltas_mes
```

Então o algoritmo recebe vários exemplos assim:

|horas|frequência|média|faltas|situação|
|--:|--:|--:|--:|---|
|6.2|90%|7.5|1|Satisfatório|
|2.5|65%|3.0|4|Precisa atenção|
|3.2|85%|6.5|1|Satisfatório|
|...|...|...|...|...|

E pensa mais ou menos:

> "Quando eu separo os alunos por determinada característica, consigo separar melhor os que são `Precisa_atencao` dos `Desempenho_satisfatorio`?"

### Por exemplo

Ele pode descobrir, **a partir dos seus dados**, uma regra parecida com:

```text
media_exercicios <= 5.1
        ↓
Precisa de atenção
```

Mas talvez descubra também:

```text
media_exercicios > 5.1
        ↓
olhar frequência
```

e depois:

```text
frequencia_percentual <= 75%
        ↓
Precisa de atenção
```

Esses números **não foram escritos por você**.

A árvore calculou quais divisões dos dados são mais úteis para separar as duas classes.

---

### E olha uma coisa interessante no SEU CSV

Eu analisei os dados que você mandou. 

A média de exercícios dos alunos está bem relacionada à situação final:

- alunos `Desempenho_satisfatorio`: média de exercícios ≈ **6,12**
    
- alunos `Precisa_atencao`: média de exercícios ≈ **4,01**
    

Então existe um padrão nos dados que permite à árvore aprender que **uma média de exercícios mais baixa tende a estar associada a "Precisa de atenção"**.

Mas atenção: isso **não significa que o algoritmo sabe que 5,0 é uma "nota baixa" no sentido humano**.

Ele sabe apenas:

> "Nos exemplos que recebi, separar os alunos em torno desse valor ajuda a prever a coluna `situacao_final`."

---

### E é por isso que essa parte é tão importante:

```python
y = df["situacao_final"]
```

Você está dizendo:

> **"Essa é a resposta que quero que a máquina aprenda a prever."**

Enquanto:

```python
x = df[variaveis]
```

diz:

> **"Essas são as informações que ela pode usar para descobrir a resposta."**

Então o fluxo é:

```text
                 DADOS DO ALUNO
                       ↓
       ┌───────────────┴───────────────┐
       ↓                               ↓
 horas de estudo                  frequência
 média exercícios                 faltas
 atividades                       etc.
       │
       ↓
  ÁRVORE DE DECISÃO
       │
       ↓
 "Quais divisões dos dados
  melhor separam as classes?"
       │
       ↓
 ┌──────────────────────┐
 │ situação_final       │
 │                      │
 │ Satisfatório         │
 │ Precisa de atenção   │
 └──────────────────────┘
```

E quando você coloca:

```python
novo_aluno = pd.DataFrame([{
    "horas_estudo_semana": 2.0,
    "frequencia_percentual": 65.0,
    "atividades_entregues": 10,
    "media_exercicios": 8.0,
    "participacao_aulas": 2,
    "acessos_plataforma_semana": 3,
    "faltas_mes": 6
}])
```

ele percorre **as regras que aprendeu na árvore** até chegar em uma folha e dizer:

```text
Precisa_atencao
```

Mesmo que a média dele seja **8**, porque a árvore pode ter aprendido que **frequência, faltas, participação etc. também são importantes**.

Se você quiser, eu posso pegar **esse seu CSV e mostrar exatamente quais regras a sua árvore criou**, tipo:

```text
frequencia <= X
   ├── faltas <= Y → Satisfatório
   └── faltas > Y → Precisa atenção
```

Aí você vai enxergar exatamente **como a IA está tomando a decisão**.