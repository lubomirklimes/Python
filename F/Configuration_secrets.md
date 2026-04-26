# Konfigurace a secrets

Tato kapitola vysvětluje rozdíl mezi konfigurací a secrety a bezpečné zacházení v Python projektech.

## 1. Concept

Konfigurace jsou běžné parametry aplikace (host, port, feature flag). Secret je citlivá hodnota (heslo, token, API key). Obojí by mělo být mimo zdrojový kód a mimo Git historii.

Lokálně se často používá `.env` soubor. V cloudu je vhodné používat secret manager (např. Key Vault) a injectovat hodnoty přes environment variables.

## 2. Example

```python
import os
from pathlib import Path

# Jednoduchý lokální fallback: načti .env pokud existuje.
env_path = Path(".env")
if env_path.exists():
    for line in env_path.read_text(encoding="utf-8").splitlines():
        if not line or line.startswith("#") or "=" not in line:
            continue
        key, value = line.split("=", 1)
        os.environ.setdefault(key.strip(), value.strip())

APP_ENV = os.getenv("APP_ENV", "dev")
DATABASE_URL = os.getenv("DATABASE_URL", "")
SECRET_KEY = os.getenv("SECRET_KEY", "")

print("APP_ENV:", APP_ENV)
print("DATABASE_URL nastaveno:", bool(DATABASE_URL))
print("SECRET_KEY nastaveno:", bool(SECRET_KEY))
```

## 3. When to use

* Když potřebujete bezpečně provozovat aplikaci napříč více prostředími.
* Když chcete oddělit deploy konfiguraci od aplikačního kódu.
* Když auditujete riziko úniku citlivých údajů.

## 4. Common mistakes

* ❌ Commit `.env` souboru s tajnými údaji do repozitáře.
* ❌ Používání stejného secretu ve všech prostředích.
* ❌ Míchání běžné konfigurace a tajných údajů bez jasných pravidel.

---

<details>
<summary>Optional: Deep dive</summary>

Dobrá praxe je rotace secretů, audit přístupu a krátká životnost tokenů. Dále pomáhá centralizovaný config kontrakt, aby byl jasný seznam povinných proměnných při startu aplikace.

</details>

---

**Další krok:** [CI/CD základy](CI_CD.md)
