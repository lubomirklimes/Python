# Cloud aplikace v Pythonu

Tato kapitola vysvětluje, jak nasazovat Flask/FastAPI aplikace v cloudu a jak pracovat s konfigurací prostředí.

## 1. Concept

Cloud runtime (App Service, Cloud Run, Container Apps, Functions host) umožňuje provozovat Python aplikaci bez ruční správy serveru. Oproti vlastnímu VM řeší platforma část provozu za vás: škálování, certifikáty, log stream, health checks.

Pro cloud deployment je důležité oddělit konfiguraci od kódu. Parametry jako `PORT`, `DATABASE_URL` nebo `ENVIRONMENT` patří do environment variables.

## 2. Example

```python
import os
from fastapi import FastAPI

app = FastAPI()

APP_NAME = os.getenv("APP_NAME", "python-cloud-app")
ENVIRONMENT = os.getenv("ENVIRONMENT", "dev")
PORT = int(os.getenv("PORT", "8000"))


@app.get("/health")
def health() -> dict[str, str]:
    return {"status": "ok", "app": APP_NAME, "env": ENVIRONMENT}


if __name__ == "__main__":
    import uvicorn

    uvicorn.run("main:app", host="0.0.0.0", port=PORT, reload=(ENVIRONMENT == "dev"))
```

## 3. When to use

* Když chcete rychle nasadit API bez správy vlastního serveru.
* Když potřebujete oddělit prostředí dev/stage/prod pomocí konfigurace.
* Když očekáváte růst provozu a potřebujete škálování.

## 4. Common mistakes

* ❌ Konfigurace natvrdo v kódu místo přes environment variables.
* ❌ Chybějící health endpoint - orchestrátor neumí správně rozhodnout o stavu aplikace.
* ❌ Míchání lokálního a produkčního režimu bez jasného přepínače prostředí.

---

<details>
<summary>Optional: Deep dive</summary>

Rozdíl runtime vs vlastní server je hlavně v odpovědnosti týmu. U runtime platíte menší flexibilitou za rychlejší operativu. U VM máte víc kontroly, ale také víc práce s patchingem, monitoringem a bezpečností.

</details>

---

**Další krok:** [Práce s databázemi v cloudu](Cloud_databaze.md)
