##### Testes de acessibilidade.
Criamos um projeto novo chamado auditoria-acessibilidade, para iniciarmos digitamos `uv init`, no arquivo .toml:
```toml
[project]
name = "auditoria-acesibilidade"
version = "0.1.0"
description = "suite de testes automatizados de usabilidadde e acessibilidade com playwrihgt"
readme = "README.md"
requires-python = ">=3.14"
dependencies = [
    "pytest>=8.3.0",
    "pytest-playwright>=0.5.0",
    "axe-playwright-python>=0.1.5",
]

[tool.pytest.ini_options]
testpaths= ["tests"]
addopts = "-v --headed"
markers= [
    "accessibility: Testes automatizados de conformidade WCAG com axe-core",
    "keyboard: Tests de navegação e operabilidade exclusica por teclado",
]

[project.scripts]
auditoria-acesibilidade = "auditoria_acesibilidade:main"

[build-system]
requires = ["uv_build>=0.12.3,<0.13.0"]
build-backend = "uv_build"

```

Depois `uv sync
Em seguida criamos a pasta tests
**test_acessibilidade.py**
```python
import pytest
from playwright.sync_api import Page
from axe_playwright_python.sync_playwright import Axe

@pytest.mark.accessibility
def test_auditoria_acessibilidade_wcag_aa(page: Page):
    # arrange
    page.goto("https://localhost:3000/formulario.html")
    # act
    axe = Axe()
    results = axe.run(
        page,
        options={
            "runOnly": {
                "type": "tag",
                "values": ["wcag2aa", "wcag21aa", "wcag2a", "wcag21a"]
                # cada value é uma regra de acessibilidade que será verificada, no caso estamos verificando as regras do WCAG 2.0 e 2.1 nível AA e A, que são as regras de acessibilidade mais comuns e importantes para garantir que o site seja acessível para pessoas com deficiência.
            }
        }
    )
    # formata a mensagem de erro para exibir apenas as violações encontradas
    violations = results.violations_count
    report = results.generate_report()
    # assert
    assert violations == 0, f"Foram encontradas {violations} violações de acessibilidade:check: \n{report}"
```
