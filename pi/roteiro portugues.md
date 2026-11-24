## **1. Abertura e apresentação da equipe (15 segundos)**
**Afonso:**
Boa noite a todos. Meu nome é Afonso, e esta é a nossa equipe: Ricardo, José, Isabele e Yan. Somos estudantes do sexto semestre e, hoje, vamos apresentar nosso projeto sobre análise termográfica da doença respiratória bovina.

## **2. Agenda (15 segundos)**

**Ricardo:**
Nossa apresentação será dividida em oito tópicos:

1. Pitch;
    
2. Problematização;
    
3. Estado da Arte;
    
4. Objetivo;
    
5. Metodologia;
    
6. Apresentação Prática;
    
7. Resultados e Discussões;
    
8. Conclusão.
## **3. Pitch (1 minuto e 24 segundos)**

## **4. Problematização (30 segundos)**
**José:**
A pecuária desempenha um papel importante na economia brasileira. Entretanto, a criação intensiva também traz desafios sanitários, especialmente a Doença Respiratória Bovina, que pode causar problemas respiratórios e comprometer o desempenho dos animais.

**Isabele:**
Nas fazendas, a saúde dos animais é monitorada principalmente pela observação visual, o que pode atrasar a identificação de doenças. Por isso, são necessárias ferramentas acessíveis para detectar problemas precocemente.

## **5. Estado da Arte (35 segundos)**
**Ricardo:**
Estudos anteriores utilizaram aprendizado de máquina e câmeras térmicas para identificar problemas respiratórios em bovinos.

**Isabele:**
Um estudo de Shu e colaboradores relacionou fortemente a temperatura do saco lacrimal esquerdo à temperatura corporal. Com base nisso, desenvolvemos o InfraCow, um protótipo acessível com aplicativo móvel que funciona offline.
## **6. Objetivo (25 segundos)**
**Yan:**
Nosso objetivo foi desenvolver o InfraCow, um sistema composto por um dispositivo IoT com sensor de temperatura infravermelho, conectado a um aplicativo mobile.

**Afonso:**
O sistema realiza a medição da temperatura na região próxima ao olho esquerdo do bovino. O InfraCow auxilia na triagem inicial, mas não substitui o diagnóstico veterinário.

## **7. Metodologia (1 minuto e 20 segundos)**
**José:**
Seguimos um procedimento padronizado para a coleta dos dados. Primeiro, o operador direciona o sensor para a região do saco lacrimal esquerdo do bovino. Em seguida, são realizadas três medições, com pequenos intervalos entre elas. Utilizamos a mediana como temperatura final.

**Yan:**
As medições são enviadas para o aplicativo por meio de um cabo e armazenadas no celular. Quando há conexão com a internet, o aplicativo envia automaticamente os dados para a API e para o banco de dados PostgreSQL.

## **8. Apresentação Prática (4 minutos e 30 segundos)**

## **9. Resultados e Discussões (1 minuto)**
**José:**
Testamos o InfraCow em bovinos e comparamos suas medições com as de um termômetro de referência. A diferença média foi inferior a um quarto de grau Celsius.

**Afonso:**
Nossos testes mostraram que a maioria das medições apresentou uma pequena variação em relação à temperatura de referência. Também realizamos testes de triagem de febre e confirmamos o funcionamento correto da API e do banco de dados.

## **10. Conclusão**
**Ricardo:**
Em conclusão, o InfraCow é uma ferramenta digital acessível que auxilia no monitoramento da saúde animal e na triagem inicial em fazendas. Gostaríamos de agradecer à FATEC Registro e ao Centro Paula Souza pelo apoio. Muito obrigado!