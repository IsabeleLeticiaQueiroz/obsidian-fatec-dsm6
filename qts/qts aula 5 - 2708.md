##### Psicologia do teste e princípios do ISTQB
---
==Mentalidade Construtiva==
- "Caminho feliz"
- Podem ser testes superficiais
==Mentalidade Crítica e Investigativa==
- Caminho mais crítico de como conduzir os testes

==**São 7 princípios:**==
**1- Presença de defeitos:**
↪︎mostra a presença dos defeitos, e não a ausência
**2- Testes Exaustivos:**
↪︎Dependendo da funcionalidade, da aplicação, é impossível testar tudo.
↪︎Não tem máquina que aguente tudo isso, portanto separação baseada em risco é a abordagem ideal.
**3- Teste antecipado:**
↪︎Economiza tempo e dinheiro, testes devem começar mais cedo possível, criando testes antes mesmo da funcionalidade, a ideia é eles realmente falharem no início justamente pela ausência das funcionalidades, porem depende da empresa, do produto, etc.
**4- Agrupamento de defeitos**
↪︎Agrupar os testes por funcionalidade, tipo...
↪︎Princípio de pareto
**5- Paradoxo do pesticida**
↪︎Não adianta ter mil testes se o sistema continua crescendo e os testes continuarem passando e a complexidade do sistema continua crescendo, se aplicar o mesmo pesticida pra sempre no mesmo lugar, os insetos criam resistência ao veneno, isso é o que acontece com os testes.
**6- Dependência do contexto**
↪︎Teste especificado para o produto, nao é algo q pode ser reaproveitado de um projeto pra outro
**7- Falácia da ausência de erros**
↪︎Encontrar e corrigir varios erros não garante a perfeição do sistema.

==Teste Baseado em Risco==
Abordagem de testes ideal, com técnicas inteligentes de amostragem:
↪︎**Particionamento de equivalência e análise do valor limite (BVA)**
↪︎**Tabelas de decisão**
↪︎**Error Guessing**

==Coisas interessantes para se testar==: tempo e data, caracteres especiais, textos, ciclos de estado, campos nulos, etc.

---
##### Prática
↪︎No cmd seguir essa sequencia:
```
cd C:\Users\fatec-dsm6\Documents\qts

uv init --app sistema-assinaturas

cd .\sistema-assinaturas

uv add "fastapi[standard]"

uv add --dev pytest pytest cov
```
↪︎No vscode, abrir o arquivo do projeto pyproject.toml, adicionar no final:
```
[tool.pytest.ini_options]
markers = [
    "unit: testes unitários"
]
```
↪︎na raiz criar uma pasta app e outra tests e dentro das duas arquivos __init__.py
↪︎o professor forneceu o arquivo main para ser colocado na pasta app:
```
# app/main.py

from typing import Optional

from fastapi import FastAPI

from pydantic import BaseModel

  

app = FastAPI(title="Motor de Faturamento de Assinaturas")

  

VALORES_PLANOS = {

    "BASICO": 50.0,

    "PRO": 150.0,

    "ENTERPRISE": 500.0

}

  

CUPONS_VALIDOS = {

    "PROMO10": ("PORCENTAGEM", 10.0),

    "DESCONTO20": ("PORCENTAGEM", 20.0),

    "BEMVINDO50": ("FIXO", 50.0)

}

  

class RequisicaoFatura(BaseModel):

    plano: str

    cupom: Optional[str] = None

    dias_atraso: int = 0

  

def calcular_faturamento(plano: str, cupom: Optional[str] = None, dias_atraso: int = 0) -> float:

    plano_upper = plano.upper()

    if plano_upper not in VALORES_PLANOS:

        raise ValueError(f"Plano invalido: {plano}")

  

    valor_base = VALORES_PLANOS[plano_upper]

    valor_com_desconto = valor_base

  

    if cupom:

        cupom_upper = cupom.upper()

        if cupom_upper in CUPONS_VALIDOS:

            tipo, taxa = CUPONS_VALIDOS[cupom_upper]

            if tipo == "PORCENTAGEM":

                valor_com_desconto = valor_base - (valor_base * (taxa / 100))

            elif tipo == "FIXO":

                valor_com_desconto = valor_base - taxa

        else:

            raise ValueError(f"Cupom invalido: {cupom}")

  

    if dias_atraso > 0:

        multa = 5.0

        juros = valor_com_desconto * (0.01 * dias_atraso)

        valor_final = valor_com_desconto + multa + juros

    else:

        valor_final = valor_com_desconto

  

    return round(valor_final, 2)
```
↪︎criar arquivo test_ai_generated.py na pasta tests, esses testes são simples feitos por uma ia, apenas para testar o caminho esperado.
```
import pytest

from app.main import calcular_faturamento

  

@pytest.mark.unit

def test_faturamento_plano_basico_sem_cupom():

    resultado = calcular_faturamento("BASICO")

    assert resultado == 50.0

    # fatura inicial de 50.0, sem cupom e sem atraso, então o valor final deve ser 50.0

  

@pytest.mark.unit

def test_faturamento_plano_pro_com_cupom_porcentagem():

    resultado = calcular_faturamento("PRO", cupom="PROMO10")

    assert resultado == 135.0

    # fatura inicial de 150.0, com cupom de 10% de desconto, então o valor final deve ser 135.0

  

@pytest.mark.unit

def test_faturamento_com_atraso():

    resultado = calcular_faturamento("BASICO", dias_atraso=10)

    assert resultado == 60.0

    # fatura inicial de 50.0, sem cupom, com 10 dias de atraso, então o valor final deve ser 50.0 + 5.0 (multa) + 5.0 (juros) = 60.0
```
↪︎ comando para testar/rodar ==**uv run pytest -v**==
↪︎ criar novo arquivo na mesma pasta "test_error_guessing.py"
```
import pytest

from app.main import calcular_faturamento

  

@pytest.mark.unit

def test_error_guessing_cupom_com_espacos_em_branco():

    resultado = calcular_faturamento("BASICO", cupom="    ")

    assert resultado == 50.0

    # fatura inicial de 50.0, com cupom de espaços em branco, então o valor final deve ser 50.0

  

@pytest.mark.unit

def test_error_guessing_dias_atraso_negativo_deve_lancar_erro():

    with pytest.raises(ValueError, match="Dias de atraso não podem ser negativos"):

        calcular_faturamento("PRO", dias_atraso=-3)

# ele espera que de um exception realemnte no sistema pra testar se ´do tipo value error e se a mensagem é "Dias de atraso não podem ser negativos"

  

@pytest.mark.unit

def test_error_guessing_desconto_nao_pode_gerar_fatura_negativa():

    resultado = calcular_faturamento("BASICO", cupom="BEMVINDO50")

    assert resultado >= 0.0

    # fatura inicial de 50.0, com cupom de desconto fixo de 50.0, então o valor final deve ser 0.0 (não pode ser negativo)

  

@pytest.mark.unit

def test_error_guessing_plano_com_espacos_extras():

    resultado = calcular_faturamento("  pro  ")

    assert resultado == 150.0

    # fatura inicial de 150.0, com plano "  pro  " (com espaços extras), então o valor final deve ser 150.0

    # vamos testar se ele remove os espaços e ainda assim reconhece o plano corretamente, alem de colocar como maiusculo
```