##### Pirâmide de testes
↪︎ Quanto mais baixo da pirâmide, mais rápido.
↪︎ Quanto mais acima, mais confiança do sistema
##### Teste unitário 
↪︎ teste simples
##### Teste de integração
↪︎ testa a comunicação entre partes, timeout, etc.
↪︎ por exemplo: teste de uso de uma API, validando resposta, tempo, etc.
↪︎ permite uma confiança maior do fluxo e a validação
==vantagens da integração==: confiança, validação, segurança.
##### Teste de UI
↪︎verifica a jornada completa do usuário, login (JWT), se entra e fecha tela, logoff...

---
##### Parte prática
↪︎ Criação de pasta "calculadora-frete"
↪︎ no terminal: uv init para iniciar e instalar dependencias iniciais
↪︎uv add "fastapi[standart]"
↪︎uv add --dev pytest pytest-cov
↪︎ o arquivo .toml é onde esta as configurações do projeto
↪︎ dentro do .toml:
	[tool.pytest.ini_options]
	markers = [
    "unit: testes unitários",
	]
↪︎ em /tests os arquivos de testes devem estar sempre em ingles (ex.: test_main)
feito em aula:
```
main.py
from fastapi import FastAPI, HTTPException, status

from pydantic import BaseModel

  

app = FastAPI(title="Calculadora de Frete Simplificada")

  

NORTE_UFS = {"AM", "RR", "RO", "AP", "PA", "AC", "TO"}

  

VALID_UFS = NORTE_UFS.union({"AL", "BA", "CE", "MA", "PB", "PB", "PI", "RN", "SE", "DF",

    "GO", "MT", "MS", "ES", "MG", "RJ", "SP", "PR", "RS", "SC"

})

  

class FreteRequest(BaseModel):

    peso: float

    uf: str

  

def calcular_frete(peso: float, uf: str) -> float:

    if peso <= 0:

        raise ValueError("O peso deve ser maior que zero")

    if peso > 30:

        raise ValueError("O peso excede o limite máximo permitido de 30kg")

  

    uf_upper = uf.strip().upper()

    if uf_upper not in VALID_UFS:

        raise ValueError("UF invalida")

  

    if peso <= 10.0:

        valor_base = 20.0

    else:

        valor_base = 50.0

  

    adicional = 15.0 if uf_upper in NORTE_UFS else 0.0

  

    return valor_base + adicional

  

# criacao de primeiro endpoint

@app.post("/frete")

def post_calcular_frete(payload: FreteRequest):

    try:

        valor = calcular_frete(peso = payload.peso, uf= payload.uf)

        return {

            "peso": payload.peso,

            "uf": payload.uf.strip().upper(),

            "valor_frete": valor

        }

    except ValueError as e:

        raise HTTPException(

            status_code=status.HTTP_400_BAD_REQUEST,

            detail=str(e)

        )
```

```
test_main
import pytest

from app.main import calcular_frete
# teste unitario

# nao é obrigatorio, é apenas uma boa prática, para sabermos a categoria e porcentagem de cobertura

@pytest.mark.unit

# o nome do test deve sempre esar ingles, ter underline e deve descrever exatamente o que faz

def test_frete_peso_invalido_zero_ou_negativo():

    with pytest.raises(ValueError, match="O peso deve ser maior que zero"):

        calcular_frete(peso= 0.0, uf="SP")

  

    with pytest.raises(ValueError, match="O peso deve ser maior que zero"):

        calcular_frete(peso=-5.0, uf="SP")

  

@pytest.mark.unit

def test_frete_excede_limite_maximo():

    assert calcular_frete(peso = 30.0, uf="SP") == 50.0

# TESTE DE BORDA, TESTANDO O PRIMEIRO DECIMO ACIMA DO LIMITE PARA EVITAR PROBLEMAS DE SISTEMAS Q

# ARREDONDEM O VALOR

    with pytest.raises(ValueError, match="O peso excede o limite máximo permitido de 30kg"):

        calcular_frete(peso= 30.1, uf="SP")

  

@pytest.mark.unit

def test_frete_uf_invalida():

    with pytest.raises(ValueError, match="UF invalida"):

        calcular_frete(peso=5.0, uf="XX")

  

@pytest.mark.unit

def teste_frete_normalizacao_uf():

    assert calcular_frete(peso=5.0, uf="        sp   ") == 20.0

    assert calcular_frete(peso=5.0, uf="am") == 35.0

  

# passando diversos testes de uma vez com uma função so

@pytest.mark.parametrize("peso, uf, esperado", [

    (0.01, "SP", 20.0),

    (10.0, "SP", 20.0),

    (10.01, "SP", 50.0),

    (30.0, "SP", 50.0),

    (5.0, "AM", 35.0),

    (15.0, "AM", 65.0),

    (5.0, "RJ", 20.0),

])

@pytest.mark.unit

def test_frete_calculos_validos_parametrizados(peso, uf, esperado):

    assert calcular_frete(peso=peso, uf=uf) == esperado
```

- [[qts aula 4 - 2108| continuacao]] 