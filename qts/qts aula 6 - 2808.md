##### Cobertura de código e métricas
↪︎Quais linhas de código foram executadas pelo menos uma vez, blocos condicionais visitados, trechos mortos ou esquecidos, lacunas.

**1- Cobertura de Instruções**
**2- Cobertura de Decisões**

---
###### Prática
- No terminal do vscode, dentro da pasta do sistema-assinaturas, um seguida do outro:
  ```
  uv add --dev pytest-cov
  
  uv run pytest --cov=app --cov-report=term-missing
  ```
- No pyproject.toml, atualizar o último tool.pytest.ini_options:
```
[tool.pytest.ini_options]
addopts = "--cov=app --cov-report=term-missing --cov-report=html"
markers = [
    "unit: testes unitários",
    "integration: testes de integração"
]
filterwarnings = [
    "ignore::starlette.exceptions.StarletteDeprecationWarning",
    "ignore::DeprecationWarning",
]
[tool.coverage.run]
omit = [
    "*/__init__.py"
]
```
- em main.py adicionar resposta fatura abaixo de requisição fatura:
```
class RequisicaoFatura(BaseModel):
...

class RespostaFatura(BaseModel):

    plano: str

    valor_base: float

    valor_com_desconto: float

    valor_multa_juros: float

    valor_final: float
```
- ainda main, em def calcular_faturamento, apagar p trecho:
```
plano_upper = plano.upper()

    if plano_upper not in VALORES_PLANOS:

        raise ValueError(f"Plano invalido: {plano}")
    valor_base = VALORES_PLANOS[plano_upper]
```
- e substituir por:
```
if not isinstance(plano, str) or not plano.strip():

        raise ValueError("O plano informado nao pode ser vazio ou nulo.")

  

    plano_normalizado = plano.strip().upper()

    if plano_normalizado not in VALORES_PLANOS:

        raise ValueError(f"Plano invalido: {plano}. Planos disponiveis: {list(VALORES_PLANOS.keys())}")

  

    if not isinstance(dias_atraso, int) or dias_atraso < 0:

        raise ValueError("Dias de atraso não podem ser negativos")

    valor_base = VALORES_PLANOS[plano_normalizado]
```
- segunda correção, em if cupom de:
```
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
```
- para:
  ```
      if cupom is not None and isinstance(cupom, str):

        cupom_normalizado = cupom.strip().upper()

        if cupom_normalizado != "":

            if cupom_normalizado not in CUPONS_VALIDOS:

                raise ValueError(f"Cupom invalido ou inexistente: {cupom}.")

  

            tipo, taxa = CUPONS_VALIDOS[cupom_normalizado]

            if tipo == "PORCENTAGEM":

                valor_com_desconto = valor_base - (valor_base * (taxa / 100))

            elif tipo == "FIXO":

                valor_com_desconto = valor_base - taxa

    if valor_com_desconto < 0:

        valor_com_desconto = 0.0
  ```

---
arquivos finais:
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

    "BEMVINDO50": ("FIXO", 50.0),

    "CREDITO100": ("FIXO", 100.0)

}

  

class RequisicaoFatura(BaseModel):

    plano: str

    cupom: Optional[str] = None

    dias_atraso: int = 0

  

class RespostaFatura(BaseModel):

    plano: str

    valor_base: float

    valor_com_desconto: float

    valor_multa_juros: float

    valor_final: float

  

def calcular_faturamento(plano: str, cupom: Optional[str] = None, dias_atraso: int = 0) -> float:

    if not isinstance(plano, str) or not plano.strip():

        raise ValueError("O plano informado nao pode ser vazio ou nulo.")

  

    plano_normalizado = plano.strip().upper()

    if plano_normalizado not in VALORES_PLANOS:

        raise ValueError(f"Plano invalido: {plano}. Planos disponiveis: {list(VALORES_PLANOS.keys())}")

  

    if not isinstance(dias_atraso, int) or dias_atraso < 0:

        raise ValueError("Dias de atraso não podem ser negativos")

    valor_base = VALORES_PLANOS[plano_normalizado]

    valor_com_desconto = valor_base

  

    if cupom is not None and isinstance(cupom, str):

        cupom_normalizado = cupom.strip().upper()

        if cupom_normalizado != "":

            if cupom_normalizado not in CUPONS_VALIDOS:

                raise ValueError(f"Cupom invalido ou inexistente: {cupom}.")

  

            tipo, taxa = CUPONS_VALIDOS[cupom_normalizado]

            if tipo == "PORCENTAGEM":

                valor_com_desconto = valor_base - (valor_base * (taxa / 100))

            elif tipo == "FIXO":

                valor_com_desconto = valor_base - taxa

    if valor_com_desconto < 0:

        valor_com_desconto = 0.0

  

    if dias_atraso > 0:

        multa = 5.0

        juros = valor_com_desconto * (0.01 * dias_atraso)

        valor_final = valor_com_desconto + multa + juros

    else:

        valor_final = valor_com_desconto

  

    return round(valor_final, 2)
```

```
#test error guessing
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

    resultado = calcular_faturamento("BASICO", cupom="CREDITO100")

    assert resultado == 0.0

    # fatura inicial de 50.0, com cupom de desconto fixo de 50.0, então o valor final deve ser 0.0 (não pode ser negativo)

  

@pytest.mark.unit

def test_error_guessing_plano_com_espacos_extras():

    resultado = calcular_faturamento("  pro  ")

    assert resultado == 150.0

    # fatura inicial de 150.0, com plano "  pro  " (com espaços extras), então o valor final deve ser 150.0

    # vamos testar se ele remove os espaços e ainda assim reconhece o plano corretamente, alem de colocar como maiudculo
```

```
#test ai generated
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

