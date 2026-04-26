# CI/CD základy

Tato kapitola shrnuje build-test-package-deploy pipeline pro Python aplikace.

## 1. Concept

CI/CD automatizuje kroky od commitu po nasazení. Typická pipeline:

- build: sestavení artefaktu (např. Docker image)
- test: unit/integration testy, lint, typová kontrola
- package: publikace artefaktu
- deploy: nasazení do cílového prostředí

Smysl je rychlá a bezpečná změna: menší riziko manuálních chyb a opakovatelné release procesy.

## 2. Example

```python
from pathlib import Path

workflow = """
name: ci
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: '3.11'
      - run: pip install -r requirements.txt
      - run: pytest -q
""".strip()

Path(".github/workflows/ci.yml").parent.mkdir(parents=True, exist_ok=True)
Path(".github/workflows/ci.yml").write_text(workflow + "\n", encoding="utf-8")
print("Ukázkový CI workflow vytvořen.")
```

## 3. When to use

* Když chcete mít testy jako povinnou bránu před nasazením.
* Když release probíhá často a manuální proces je nespolehlivý.
* Když potřebujete auditovat, co bylo nasazeno a kdy.

## 4. Common mistakes

* ❌ Pipeline nasazuje bez testů nebo bezpečnostních kontrol.
* ❌ Chybí separace prostředí (dev/stage/prod) a schvalovací pravidla.
* ❌ Nezachytávání artefaktů - nejde dohledat, co přesně běželo v produkci.

---

<details>
<summary>Optional: Deep dive</summary>

Větší týmy přidávají canary/blue-green nasazení, rollback strategie a policy-as-code. Důležité je, aby pipeline byla rychlá; pomalá CI se obchází a ztrácí smysl.

</details>

---

**Další krok:** [Observability: logy, metriky a tracing](Observability.md)
