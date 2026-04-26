# FastAPI

Tato kapitola ukazuje moderní API vývoj ve FastAPI s asynchronními endpointy a automatickou validací.

## 1. Concept

FastAPI je framework zaměřený na API. Kombinuje routing, typové anotace a validaci dat přes Pydantic modely. Výsledkem je rychlý vývoj, silná kontrola vstupů a automatická OpenAPI dokumentace.

Async endpointy (`async def`) jsou vhodné hlavně pro I/O-bound práci (síť, databáze). Samotné `async` ale automaticky nezrychlí CPU-bound výpočty.

## 2. Example

```python
from fastapi import FastAPI, HTTPException
from pydantic import BaseModel, Field

app = FastAPI()


class ItemIn(BaseModel):
    name: str = Field(min_length=1, max_length=100)
    price: float = Field(gt=0)


@app.get("/health")
async def health() -> dict[str, str]:
    return {"status": "ok"}


@app.post("/items")
async def create_item(item: ItemIn) -> dict[str, str | float]:
    if item.name.lower() == "zakazane":
        raise HTTPException(status_code=400, detail="Neplatný název položky")
    return {"name": item.name, "price": item.price}
```

## 3. When to use

* Když tvoříte moderní REST API s důrazem na validaci a dokumentaci.
* Když potřebujete zvládat více paralelních I/O operací.

## 4. Common mistakes

* ❌ Všechno psát async bez znalosti trade-offů – CPU-heavy kód tím nezrychlíte.
* ❌ Obcházet Pydantic validaci ručním parsováním JSON – ztrácíte hlavní výhodu frameworku.

---

<details>
<summary>Optional: Deep dive</summary>

FastAPI generuje OpenAPI schema z anotací, takže konzistence typů přímo ovlivňuje kvalitu API dokumentace. V produkci se vyplatí doplnit centralizovaný error handler a logging middleware.

</details>

---

**Další krok:** [REST API](REST_API.md)
