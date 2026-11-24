##### Distância Euclidiana
- Distância de uma linha reta entre 2 pontos

**D = √[(X2-X1)² + (Y2-Y1)²]**

<div align="center">  
<img src="imgs/EUCLIDIANA.png" width="438">  
</div>

==Onde usar?==
- KNN
  ↪︎Problemas com atributos numéricos

> [!NOTE]
> **Questão de integradora? Ver gráfico e calcular distância euclidiana.**

---
##### Distância Manhatan
- Medir o deslocamento horizontal e vertical
- Ex.: Pessoa andando por duas quadras sem utilizar diagonais
- ou seja, movimentos em linhas retas verticais e/ou horizontais

Como descobrir o valor de A e de B, por exemplo?
**A(X, Y) = (2,2)**
**B(X, Y) = (5,7)**
<div align="center">  
<img src="imgs/MANHATAN.png" width="438">  
</div>
---
##### Distância Minkowski
- Família de distâncias
- Generaliza outras medidas por meio de um parametro (P)
- MUDANÇA CONFORME POSIÇÃO
![[formulamikowsi.png]]
- **\(D\):** distância entre os pontos A e B.
- **\(A_i\):** coordenada do ponto A.
- **\(B_i\):** coordenada do ponto B.
- **E:** indica que devemos somar os resultados.
- **\(p\):** parâmetro que determina o tipo de distância.
- **\(n\):** quantidade de dimensões ou coordenadas.

![[Pasted image 20261029203348.png]]

---
**Próx. Aula**
- Seno e Cosseno
- Similaridade de Hering