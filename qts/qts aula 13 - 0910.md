##### Ineficiência do Código de IA e Testes de Carga com Locust

###### 1. Ineficiência do código gerado por IA
Códigos gerados por Inteligência Artificial podem funcionar corretamente, mas nem sempre apresentam bom desempenho. Podem conter operações desnecessárias, consultas ineficientes ou consumo excessivo de recursos.
Por isso, além de verificar se o código funciona, é importante avaliar sua **eficiência e seu desempenho**.

> [!important] Conceito-chave  
> Um código pode estar funcionalmente correto e, mesmo assim, ser ineficiente em termos de tempo de execução e utilização de recursos.

###### 2. Subcaracterísticas de desempenho
A característica de qualidade relacionada ao desempenho possui três subcaracterísticas centrais:

###### Comportamento temporal (_Time Behaviour_)
Avalia o tempo necessário para executar uma operação e a capacidade de responder dentro de um intervalo esperado.
**Exemplos:**

- Tempo de resposta de uma página.
    
- Duração de uma consulta ao banco de dados.
    
- Tempo necessário para processar uma requisição.
    

###### Utilização de recursos (_Resource Utilization_)
Avalia a quantidade de recursos computacionais consumidos durante a execução.
**Exemplos:**
- Uso de CPU.
    
- Consumo de memória RAM.
    
- Utilização de rede e conexões com o banco de dados.
    

###### Capacidade (_Capacity_)
Avalia os limites de um sistema em relação à quantidade de usuários, requisições, transações ou dados que consegue suportar, mantendo os níveis de desempenho esperados.
**Exemplos:**
- Quantidade máxima de usuários simultâneos.
    
- Número de requisições processadas por segundo.
    
- Quantidade de conexões suportadas.
    

> [!tip] Para memorizar  
> **Tempo = quão rápido; Recursos = quanto consome; Capacidade = quanto suporta.**

###### 3. Testes de desempenho
Os testes de desempenho verificam como um sistema se comporta sob diferentes condições de utilização.

###### Teste de carga (_Load Testing_)
Verifica o comportamento do sistema sob uma carga esperada ou planejada.
**Objetivo:** avaliar se o sistema suporta o volume previsto de usuários e requisições.
_Exemplo:_ simular 100 usuários acessando um site ao mesmo tempo.

###### Teste de estresse (_Stress Testing_)

Submete o sistema a uma carga superior à esperada para identificar seus limites e observar como ele se comporta quando sobrecarregado.
**Objetivo:** descobrir o ponto de saturação e avaliar a recuperação após a sobrecarga.
_Exemplo:_ aumentar progressivamente o número de usuários até que o sistema apresente erros ou tempos de resposta excessivos.

###### Teste de pico (_Spike Testing_)
Avalia a reação do sistema a aumentos ou reduções bruscas de carga em um curto período.
**Objetivo:** verificar se o sistema consegue lidar com mudanças repentinas na demanda.
_Exemplo:_ um site normalmente recebe 50 usuários simultâneos, mas passa repentinamente a receber 1.000.

###### Teste de resistência (_Soak Testing_ ou _Endurance Testing_)
Mantém uma carga relativamente constante durante um período prolongado.
**Objetivo:** identificar problemas que aparecem com o tempo, como vazamentos de memória, degradação de desempenho e esgotamento de recursos.
_Exemplo:_ manter 200 usuários simulados acessando um sistema durante várias horas.

###### 4. Ferramenta Locust
O **Locust** é uma ferramenta de código aberto utilizada para realizar testes de carga em aplicações, simulando usuários que executam ações e enviam requisições.
Com ele, é possível avaliar:
- Quantidade de requisições por segundo.
    
- Tempo de resposta.
    
- Taxa de falhas.
    
- Comportamento do sistema conforme o número de usuários aumenta.
    

###### Como funciona?
1. Define-se o comportamento que os usuários simulados executarão.
    
2. Configura-se a quantidade de usuários e a velocidade de entrada deles.
    
3. O Locust executa as requisições.
    
4. Os resultados são analisados para identificar gargalos e limitações.
    

> [!warning] Atenção  
> O Locust simula carga, mas não corrige automaticamente o código. Os resultados ajudam a identificar problemas que precisam ser investigados e solucionados.

###### 5. Quadro comparativo para a prova

|Tipo de teste|Principal objetivo|
|---|---|
|**Carga**|Verificar o desempenho sob uma carga esperada.|
|**Estresse**|Identificar limites e comportamento sob sobrecarga.|
|**Pico**|Avaliar reações a mudanças bruscas de carga.|
|**Resistência**|Identificar problemas após uso prolongado.|

##### 6. Revisão rápida
- Sei explicar por que um código gerado por IA pode ser funcional, mas ineficiente.
    
- Sei diferenciar comportamento temporal, utilização de recursos e capacidade.
    
- Sei distinguir teste de carga, estresse, pico e resistência.
    
- Sei explicar para que serve o Locust.
    
- Sei identificar qual teste utilizar em cada situação.
    

###### 7. Questões de fixação
**1.** Um sistema demora muito para responder a uma requisição. Qual subcaracterística de desempenho está relacionada ao problema?
**2.** Uma aplicação apresenta consumo excessivo de memória RAM. Qual subcaracterística deve ser analisada?
**3.** Uma loja virtual deseja verificar se suporta o número de acessos previsto para uma promoção. Qual teste deve realizar?
**4.** Um sistema recebe repentinamente dez vezes mais acessos que o habitual. Qual tipo de teste avalia esse cenário?
**5.** Uma aplicação funciona normalmente no início, mas começa a apresentar lentidão após várias horas. Qual teste pode revelar esse problema?
**6.** Qual é a função do Locust em uma estratégia de testes de desempenho?

---

**Palavras-chave:** Qualidade de Software · Desempenho · Eficiência · Teste de Carga · Teste de Estresse · Spike Testing · Soak Testing · Locust