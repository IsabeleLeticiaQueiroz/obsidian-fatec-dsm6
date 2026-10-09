###### Aprendizagem de Máquina — Fundamentos

###### 1. Introdução à Aprendizagem de Máquina

A **Aprendizagem de Máquina (Machine Learning)** é uma área da Inteligência Artificial que permite que sistemas aprendam padrões a partir de dados para realizar previsões ou tomar decisões, sem que todas as regras precisem ser programadas explicitamente.

###### Tipos de aprendizagem

- **Supervisionada:** o modelo aprende com exemplos que possuem respostas conhecidas (rótulos).
    
- **Não supervisionada:** o modelo busca padrões ou agrupamentos em dados sem respostas previamente fornecidas.
    
- **Por reforço:** o agente aprende por meio de interações com um ambiente, recebendo recompensas ou penalidades.
    

> [!tip] Para a prova  
> Na aprendizagem supervisionada, os dados de treinamento possuem os atributos de entrada e a resposta esperada.

###### 2. Representação matemática

Uma forma simplificada de representar um modelo de aprendizagem de máquina é:

y=f(x)y = f(x)

Em que:

- **xx — atributos (_features_):** informações utilizadas para realizar uma previsão.
    
- **ff — função ou modelo:** mapeamento aprendido a partir dos dados.
    
- **yy — variável de saída:** resultado ou valor que se deseja prever.
    

###### Exemplo

Imagine um modelo que estima o preço de uma casa.

- xx: área, localização e número de quartos.
    
- ff: modelo que relaciona essas características ao preço.
    
- yy: preço estimado da casa.
    

> [!important] Atenção  
> Em aprendizagem supervisionada, yy costuma representar o alvo (_target_). Durante o treinamento, o modelo aprende uma aproximação da relação entre xx e yy. Em previsões, é comum representar a saída estimada por y^\hat{y}.

###### 3. Convenções de K-NN: K5, K10 e K-N

O **K-NN (K-Nearest Neighbors)**, ou algoritmo dos _K vizinhos mais próximos_, é um método utilizado principalmente em classificação e regressão.

O parâmetro kk representa a quantidade de vizinhos considerados para determinar o resultado.

- **K5:** considera os 5 vizinhos mais próximos.
    
- **K10:** considera os 10 vizinhos mais próximos.
    
- **K-N:** notação genérica em que kk representa uma quantidade de vizinhos definida para o modelo.
    

###### Como funciona?

Na classificação, o algoritmo identifica os kk exemplos mais próximos de uma nova observação e utiliza seus rótulos para determinar a classe prevista, geralmente por votação.

> [!example] Exemplo  
> Um modelo precisa classificar uma fruta. Entre os 5 vizinhos mais próximos, 4 são maçãs e 1 é uma pera. Pela votação majoritária, a previsão será **maçã**.

> [!warning] Cuidado na prova  
> Aumentar o valor de kk altera a quantidade de vizinhos considerados e pode mudar a classificação. Um kk pequeno pode ser mais sensível a ruídos; um kk grande pode suavizar demais as diferenças entre classes.

**Ponto para confirmar com o professor:** a expressão “convenções consagradas de K5, K10 e KN” pode ser uma referência específica ao material da disciplina. No K-NN convencional, esses termos indicam quantidades de vizinhos, mas vale conferir se a aula utiliza alguma definição própria.

###### 4. Data Leakage (Vazamento de Dados)

**Data leakage** ocorre quando informações que não deveriam estar disponíveis durante o treinamento acabam sendo utilizadas pelo modelo, permitindo que ele obtenha resultados artificialmente bons.

Isso pode acontecer, por exemplo, quando informações do conjunto de teste influenciam o treinamento ou quando um atributo contém indiretamente a resposta que o modelo deveria prever.

###### Consequências

- Métricas de avaliação artificialmente elevadas.
    
- Falsa impressão de que o modelo generaliza bem.
    
- Previsões ruins quando o modelo é utilizado em situações reais.
    
- Decisões baseadas em avaliações pouco confiáveis.
    

###### Como evitar?

1. Separar os dados de treinamento e teste corretamente.
    
2. Ajustar transformações e pré-processamentos utilizando apenas os dados de treinamento.
    
3. Evitar atributos que revelem diretamente a resposta ou informações futuras indisponíveis no momento da previsão.
    
4. Utilizar validação adequada para escolher e ajustar o modelo.
    

> [!important] Regra de ouro  
> **O conjunto de teste não deve participar do treinamento nem da escolha do modelo.** Ele deve ser reservado para uma avaliação final e independente.

> [!example] Exemplo de vazamento  
> Você quer prever se um paciente terá determinada doença, mas utiliza como atributo um resultado de exame que só é disponibilizado depois do diagnóstico. O modelo pode parecer muito preciso, mas está utilizando uma informação que não estaria disponível no momento real da previsão.

## 5. Revisão para a prova

-  Sei diferenciar aprendizagem supervisionada, não supervisionada e por reforço.
    
-  Sei explicar o significado de xx, ff e yy em y=f(x)y=f(x).
    
-  Sei identificar o significado de K5, K10 e K-N no K-NN.
    
-  Sei definir _data leakage_ e reconhecer exemplos.
    
-  Sei explicar as consequências do vazamento de dados.
    
-  Sei identificar práticas corretas para evitar _data leakage_.
    

## 6. Questões de fixação

**1.** Em um modelo representado por y=f(x)y=f(x), o que representa xx?

**2.** Em um K-NN com k=10k=10, quantos vizinhos são considerados?

**3.** Um modelo utiliza informações do conjunto de teste durante o treinamento. Qual problema pode ocorrer?

**4.** Por que o _data leakage_ pode produzir métricas de avaliação enganosas?

**5.** Qual é a regra fundamental para evitar o uso indevido do conjunto de teste?

---

**Palavras-chave:** Machine Learning · Aprendizagem Supervisionada · Features · Target · K-NN · Data Leakage