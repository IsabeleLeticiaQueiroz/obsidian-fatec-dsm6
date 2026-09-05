##### Particionamento de Equivalência e Análise de Valor Limite
↪︎Partições Válidas
↪︎partições Inválidas

---
###### Prática - Motor-credito
- no terminal da pasta, rode uma linha de cada vez:
```terminal
uv init

uv add "fastapi[standard]" pydantic

uv add --dev pytest pytest-cov httpx

```
- em pyproject.toml, no trecho:
```
description = "Add your description here"

```
substituir por:
```
description = "Motor de concessão e análise de limite de crédito"

```
- depois de dependency-groups, adicionar:
```
[tool.uv]
package = false

[tool.pytest.ini_options]
addopts = "--cov=app --cov-branch --cov-report=term-missing --cov-report=html"
markers = [
    "unit: testes unitarios isolados",
    "integration: testes de integracao de API",
    "blackbox: testes de caixa preta (EP e BVA)",
    "whitebox: testes de caixa branca (cobertura estrutural)",
]
filterwarnings = [
    "ignore::starlette.exceptions.StarletteDeprecationWarning",
    "ignore::DeprecationWarning",
]

[tool.coverage.run]
omit = [
    "*/__init__.py",
]
```
- criar uma pasta app e uma tests, ambas na raiz do projeto
- dentro de app, criar schemas.py, o codigo foi entregue ja feito pelo professor:
```python
from pydantic import BaseModel, Field

from typing import Literal

class SolicitacaoCredito(BaseModel):

    idade: int = Field(..., description="Idade do solicitante em anos")

    renda_mensal: float = Field(..., description="Renda liquida mensal comprovada")

    score_serasa: int = Field(..., description="Pontuacao de credito de 0 a 1000")

    valor_solicitado: float = Field(..., description="Valor total do emprestimo solicitado")

    quantidade_parcelas: int = Field(..., description="Quantidade de parcelas mensais")

    possui_restricao_nome: bool = Field(default=False, description="Indica se ha restricao ativa no CPF")

    tempo_relacionamento_anos: int = Field(default=0, description="Tempo de relacionamento com o banco em anos")

  
class ResultadoAnaliseCredito(BaseModel):

    status: Literal["APROVADO", "REPROVADO"]

    categoria_risco: str

    limite_maximo_aprovado: float

    taxa_juros_mensal: float

    valor_parcela: float

    motivo: str
```
- criar um service.py também dentro de app, também entregue completo:
```python
from app.schemas import SolicitacaoCredito, ResultadoAnaliseCredito

  
  

def validar_dados_entrada(solicitacao: SolicitacaoCredito) -> None:

    """Aplica validacoes defensivas e limites de fronteira sobre os dados de entrada."""

    if solicitacao.idade < 18:

        raise ValueError("Idade minima permitida e 18 anos.")

    if solicitacao.idade > 75:

        raise ValueError("Idade maxima permitida e 75 anos.")

  

    if solicitacao.score_serasa < 0 or solicitacao.score_serasa > 1000:

        raise ValueError("Score Serasa deve estar entre 0 e 1000.")

  

    if solicitacao.renda_mensal <= 0:

        raise ValueError("Renda mensal deve ser maior que zero.")

  

    if solicitacao.valor_solicitado <= 0:

        raise ValueError("Valor solicitado deve ser maior que zero.")

  

    if solicitacao.quantidade_parcelas < 6:

        raise ValueError("Quantidade minima de parcelas e 6.")

    if solicitacao.quantidade_parcelas > 72:

        raise ValueError("Quantidade maxima de parcelas e 72.")

  

    if solicitacao.tempo_relacionamento_anos < 0:

        raise ValueError("Tempo de relacionamento nao pode ser negativo.")

  
  

def avaliar_solicitacao_credito(solicitacao: SolicitacaoCredito) -> ResultadoAnaliseCredito:

    """Executa o motor de regras de analise, score e concessao de credito."""

    validar_dados_entrada(solicitacao)

  

    # 1. Regra de Restricao Cadastral

    if solicitacao.possui_restricao_nome:

        return ResultadoAnaliseCredito(

            status="REPROVADO",

            categoria_risco="RESTRICAO",

            limite_maximo_aprovado=0.0,

            taxa_juros_mensal=0.0,

            valor_parcela=0.0,

            motivo="Restricao cadastral ativa no CPF."

        )

  

    # 2. Categorizacao por Faixa de Score

    if solicitacao.score_serasa < 300:

        return ResultadoAnaliseCredito(

            status="REPROVADO",

            categoria_risco="ALTO_RISCO_REPROVADO",

            limite_maximo_aprovado=0.0,

            taxa_juros_mensal=0.0,

            valor_parcela=0.0,

            motivo="Score de credito insuficiente para concessao."

        )

    elif solicitacao.score_serasa <= 599:

        categoria = "BRONZE"

        multiplicador = 2.0

        taxa_base = 0.085

    elif solicitacao.score_serasa <= 799:

        categoria = "PRATA"

        multiplicador = 4.0

        taxa_base = 0.045

    else:

        categoria = "OURO"

        multiplicador = 8.0

        taxa_base = 0.020

  

    limite_maximo = round(solicitacao.renda_mensal * multiplicador, 2)

  

    # 3. Calculo de Bonificacoes e Taxa Ajustada

    taxa_ajustada = taxa_base

  

    if solicitacao.tempo_relacionamento_anos >= 5 and solicitacao.score_serasa >= 600:

        taxa_ajustada -= 0.005

  

    if solicitacao.quantidade_parcelas <= 12:

        taxa_ajustada -= 0.002

  

    if taxa_ajustada < 0.014:

        taxa_ajustada = 0.014

  

    taxa_percentual = round(taxa_ajustada * 100, 2)

  

    # 4. Verificacao de Limite Maximo

    if solicitacao.valor_solicitado > limite_maximo:

        return ResultadoAnaliseCredito(

            status="REPROVADO",

            categoria_risco=categoria,

            limite_maximo_aprovado=limite_maximo,

            taxa_juros_mensal=taxa_percentual,

            valor_parcela=0.0,

            motivo=f"Valor solicitado excede o limite pre-aprovado de R$ {limite_maximo:.2f}."

        )

  

    # 5. Calculo da Parcela e Comprometimento de Renda

    total_com_juros = solicitacao.valor_solicitado * (1.0 + (taxa_ajustada * solicitacao.quantidade_parcelas))

    valor_parcela = round(total_com_juros / solicitacao.quantidade_parcelas, 2)

    comprometimento_maximo = round(solicitacao.renda_mensal * 0.30, 2)

  

    if valor_parcela > comprometimento_maximo:

        return ResultadoAnaliseCredito(

            status="REPROVADO",

            categoria_risco=categoria,

            limite_maximo_aprovado=limite_maximo,

            taxa_juros_mensal=taxa_percentual,

            valor_parcela=valor_parcela,

            motivo=f"Parcela de R$ {valor_parcela:.2f} excede 30% da renda mensal (R$ {comprometimento_maximo:.2f})."

        )

  

    return ResultadoAnaliseCredito(

        status="APROVADO",

        categoria_risco=categoria,

        limite_maximo_aprovado=limite_maximo,

        taxa_juros_mensal=taxa_percentual,

        valor_parcela=valor_parcela,

        motivo="Credito aprovado com sucesso."

    )
```
  - criar main.py em app:
```python
from fastapi import FastAPI

from app.schemas import SolicitacaoCredito, ResultadoAnaliseCredito

from app.service import avaliar_solicitacao_credito

  

app = FastAPI(

    title="Motor de Crédito API",

    description="API para análise de crédito e concessão de empréstimos.",

    version="1.0.0"

)

  

@app.get("/health")

def health_check():

    """Endpoint de verificação de saúde da API."""

    return {"status": "healthy", "service": "Motor de Crédito API"}

  

@app.post("/credito/avaliar", response_model=ResultadoAnaliseCredito)

def endpoint_avaliar_credito(solicitacao: SolicitacaoCredito):

    """processa a solicitacao de credito e retorna o resultado da analise."""

    try:

        resultado = avaliar_solicitacao_credito(solicitacao)

        return resultado

    except ValueError as exc:

        return HTTPException(status_code=422, detail=str(exc))
```
- criar tests\conftest.py:
```python
import pytest

from fastapi.testclient import TestClient

from app.main import app

  

# o fixture cria um cliente de teste para a aplicação FastAPI, reutilizando o mesmo para todos os testes no módulo

@pytest.fixture

def client() -> TestClient:

    """ Fixture fornece um cliente http para testes da API FastAPI. """

    return TestClient(app)

def pytest_collectoin_modifyitems(items):

    """ Garante ordenacao de testes unitarios antes de integração"""

    order_priority = {"unit": 1, "blackbox": 2, "whitebox": 3, "integration": 4}

  

    def get_priority(item):

        for mark in item.iter_markers():

            if mark.name in order_priority:

                return order_priority[mark.name]

            return 99

    items.sort(key=get_priority)
```
- criar tests\test_caixa_preta.py:
```python
import pytest

from app.schemas import SolicitacaoCredito

from app.service import avaliar_solicitacao_credito

  

# particionamento de equivalencia e valor limite: idade

  

@pytest.mark.unit

@pytest.mark.blackbox

@pytest.mark.parametrize(

    "idade_invalida, mensagem_esperada",

    [

        (17, "Idade minima permitida e 18 anos."),

        (0, "Idade minima permitida e 18 anos."),

        (-5, "Idade minima permitida e 18 anos."),

        (76, "Idade maxima permitida e 75 anos."),

        (100, "Idade maxima permitida e 75 anos."),

    ],

    ids = ["bva_17_abaixo_limite", "zero_idade", "negativo_idade", "bva_76_acima_limite", "cem_anos"]

)

def test_bva_idade_deve_lancar_erro(idade_invalida: int, mensagem_esperada: str):

    """ Testa a validacao de idade com particionamento de equivalencia e valor limite. """

    solicitacao = SolicitacaoCredito(

        idade=idade_invalida,

        renda_mensal=5000.0,

        score_serasa=700,

        valor_solicitado=5000.0,

        quantidade_parcelas=12

    )

    with pytest.raises(ValueError, match=mensagem_esperada):

        avaliar_solicitacao_credito(solicitacao)
```
- NÃO ESQUEÇA DE POR "__init__.py" tanto em app quanto em test, e nao esqueça de por underline!!!