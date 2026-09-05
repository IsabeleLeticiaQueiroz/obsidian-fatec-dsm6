##### Continuação do motor-credit
↪︎test_caixa_preta.py
```python
import pytest

from app.schemas import SolicitacaoCredito

from app.service import avaliar_solicitacao_credito

# Particionamento de Equivalência e Valor Limite : IDADE
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

    ids=["bva_17_abaixo_limite", "zero_idade", "negativo_idade", "bva_76_acima_limite", "cem_anos"]

)

def test_bva_idade_invalida_deve_lancar_erro(idade_invalida: int, mensagem_esperada: str):

    solicitacao = SolicitacaoCredito(

        idade=idade_invalida,

        renda_mensal=5000.0,

        score_serasa=700,

        valor_solicitado=5000.0,

        quantidade_parcelas=12

    )

    with pytest.raises(ValueError, match=mensagem_esperada):

        avaliar_solicitacao_credito(solicitacao)

  
  

@pytest.mark.unit

@pytest.mark.blackbox

@pytest.mark.parametrize(

    "idade_valida",

    [18, 19, 45, 74, 75],

    ids=["bva_18_minimo_exato", "bva_19_um_acima_minimo", "particao_valida_media", "bva_74_um_abaixo_maximo", "bva_75_maximo_exato"]

)

def test_bva_idade_valida_deve_processar_com_sucesso(idade_valida: int) :

    """ Valida valores limites validos para o campo idade """

    solicitacao = SolicitacaoCredito(

        idade=idade_valida,

        renda_mensal=5000.0,

        score_serasa=700,

        valor_solicitado=5000.0,

        quantidade_parcelas=12

    )

    resultado = avaliar_solicitacao_credito(solicitacao)

    assert resultado.status == "APROVADO"

  
  

# 2. Particionamento de Equivalência e valor limite: SCORE SERASA

@pytest.mark.unit

@pytest.mark.blackbox

@pytest.mark.parametrize(

    "score_invalido",

    [-1, -100, 1001, 1500],

    ids=["bva_menos_um", "negativo_extremo", "bva_1001", "acima_extremo"]

)

def test_bva_score_invalido_deve_lancar_erro(score_invalido: int):

    """ Valida limites externos invalidos para score de credito. """

    solicitacao = SolicitacaoCredito(

            idade=30,

            renda_mensal=5000.0,

            score_serasa=score_invalido,

            valor_solicitado=5000.0,

            quantidade_parcelas=12

        )

    with pytest.raises(ValueError, match="Score Serasa deve estar entre 0 e 1000."):

        avaliar_solicitacao_credito(solicitacao)

  
  

@pytest.mark.unit

@pytest.mark.blackbox

@pytest.mark.parametrize(

    "score, categoria_esperada, multiplicador_esperado",

    [

        (0, "ALTO_RISCO_REPROVADO", 0.0),

        (299, "ALTO_RISCO_REPROVADO", 0.0),

        (300, "BRONZE", 2.0),

        (599, "BRONZE", 2.0),

        (600, "PRATA", 4.0),

        (799, "PRATA", 4.0),

        (800, "OURO", 8.0),

        (1000, "OURO", 8.0),

    ],

    ids=[

        "bva_score_0_reprovado",

        "bva_score_299_reprovado",

        "bva_score_300_bronze",

        "bva_score_599_bronze",

        "bva_score_600_prata",

        "bva_score_799_prata",

        "bva_score_800_ouro",

        "bva_score_1000_ouro",

    ]

)

def test_bva_transicao_faixas_score(score: int, categoria_esperada: str, multiplicador_esperado: float):

    """Valida com precisao matematica todos os pontos de fronteira entre as faixas de score."""

    renda = 4000.0

    solicitacao = SolicitacaoCredito(

        idade=30,

        renda_mensal=renda,

        score_serasa=score,

        valor_solicitado=1000.0,

        quantidade_parcelas=12

    )

    resultado = avaliar_solicitacao_credito(solicitacao)

  

    assert resultado.categoria_risco == categoria_esperada

    assert resultado.limite_maximo_aprovado == round(renda * multiplicador_esperado, 2)

  

# 3. Particionamento de Equivalência: PARCELAS E DADOS CADASTRAIS

  

@pytest.mark.unit

@pytest.mark.blackbox

@pytest.mark.parametrize(

    "parcelas_invalidas, mensagem_esperada",

    [

        (5, "Quantidade minima de parcelas e 6."),

        (0, "Quantidade minima de parcelas e 6."),

        (73, "Quantidade maxima de parcelas e 72."),

    ],

    ids=["bva_5_parcelas", "zero_parcelas", "bva_73_parcelas"]

)

def test_bva_parcelas_invalidas_deve_lancar_erro(parcelas_invalidas: int, mensagem_esperada: str):

    """Valida fronteiras e limites de quantidade de parcelas."""

    solicitacao = SolicitacaoCredito(

        idade=30,

        renda_mensal=5000.0,

        score_serasa=700,

        valor_solicitado=2000.0,

        quantidade_parcelas=parcelas_invalidas

    )

    with pytest.raises(ValueError, match=mensagem_esperada):

        avaliar_solicitacao_credito(solicitacao)

  

@pytest.mark.unit

@pytest.mark.blackbox

def test_particao_invalida_renda_e_valor_negativos():

    """Valida particoes invalidas para renda, valor e tempo de relacionamento."""

    with pytest.raises(ValueError, match="Renda mensal deve ser maior que zero."):

        avaliar_solicitacao_credito(SolicitacaoCredito(

            idade=30, renda_mensal=0.0, score_serasa=700, valor_solicitado=1000.0, quantidade_parcelas=12

        ))

  

    with pytest.raises(ValueError, match="Valor solicitado deve ser maior que zero."):

        avaliar_solicitacao_credito(SolicitacaoCredito(

            idade=30, renda_mensal=5000.0, score_serasa=700, valor_solicitado=-10.0, quantidade_parcelas=12

        ))

  

    with pytest.raises(ValueError, match="Tempo de relacionamento nao pode ser negativo."):

        avaliar_solicitacao_credito(SolicitacaoCredito(

            idade=30, renda_mensal=5000.0, score_serasa=700, valor_solicitado=1000.0,

            quantidade_parcelas=12, tempo_relacionamento_anos=-1

        ))

  

@pytest.mark.unit

@pytest.mark.blackbox

def test_regra_restricao_cadastral_reprova_automaticamente():

    """ Valida se solicitante com restrição ativa é reprovado mesmo com score e renda altos. """

    solicitacao = SolicitacaoCredito(

        idade=35,

        renda_mensal=10000.0,

        score_serasa=950,

        valor_solicitado=5000.0,

        quantidade_parcelas=12,

        possui_restricao_nome=True

    )

    resultado = avaliar_solicitacao_credito(solicitacao)

    assert resultado.status == "REPROVADO"

    assert resultado.categoria_risco == "RESTRICAO"

    assert resultado.limite_maximo_aprovado == 0.0

    assert "Restricao cadastral" in resultado.motivo
```
↪︎test_caixa_branca.py
```python
import pytest

from fastapi.testclient import TestClient

from app.schemas import SolicitacaoCredito

from app.service import avaliar_solicitacao_credito

# teste de caixa branca: ramificacao de bonificacao e taxa

@pytest.mark.unit

@pytest.mark.whitebox

def test_ramificacao_desconto_fidelidade_e_prazo_curto():

    solicitacao = SolicitacaoCredito(

        idade=30,

        renda_mensal=6000.0,

        score_serasa=700,

        valor_solicitado=2000.0,

        quantidade_parcelas=10,

        tempo_relacionamento_anos=6

    )

    resultado = avaliar_solicitacao_credito(solicitacao)

    assert resultado.status == "APROVADO"

    assert resultado.taxa_juros_mensal == 3.8

# testes de integracao http

@pytest.mark.integration

def test_endpoint_health(client: TestClient):

    """valida o endpoint de health check"""

    response = client.get("/health")

    assert response.status_code == 200

    assert response.json()["status"] == "healthy"  

  

@pytest.mark.integration

def test_endpoint_avaliar_credito_sucesso(client: TestClient):

    """valida requisicao """

    payload = {

        "idade": 30,

        "renda_mensal": 5000.0,

        "score_serasa": 850,

        "valor_solicitado": 10000.0,

        "quantidade_parcelas": 24,

        "possui_restricao_nome": False,

        "tempo_relacionamento_anos": 5

    }

    response = client.post("/credito/avaliar", json=payload)

    assert response.status_code == 200

    dados = response.json()

    assert dados["status"] == "APROVADO"

    assert dados["categoria_risco"] == "OURO"
```
