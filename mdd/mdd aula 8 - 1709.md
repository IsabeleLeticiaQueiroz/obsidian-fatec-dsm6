==eu faltei na ultima aula, mas tudo que for colocado aqui se refere a ultima aula e a atual tambem==
##### Aula 17/09
codigos pasta md

---
###### One-hot
Transforma categorias em colunas com DEL para modelos de Machine Learning consigam trabalhar com ela.
Exemplo:

| Cor      | Vermelho | Azul | Verde |
| -------- | -------- | ---- | ----- |
| Vermelho | 1        | 0    | 0     |
| Azul     | 0        | 1    | 0     |
| Verde    | 0        | 0    | 1     |
Cada categoria vira uma coluna, onde 1 indica que aquela categoria está presente e 0 indica que está ausente.
Assim o modelo não irá interpretar "18-24", "25-34"... Como números de uma ordem matemática arbitrária.