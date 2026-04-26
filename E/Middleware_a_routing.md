# Middleware a routing

Tato kapitola vysvětluje, jak funguje routing a middleware ve webových aplikacích a API.

## 1. Concept

Routing mapuje URL + HTTP metodu na konkrétní handler. Middleware je mezivrstva, která zpracuje request/response kolem handleru (např. logging, CORS, autentizace, trace ID, měření latence).

Dobře navržený middleware centralizuje společnou logiku, takže ji nemusíte kopírovat do každého endpointu.

## 2. Example

```python
from fastapi import FastAPI, Request
import time

app = FastAPI()


@app.middleware("http")
async def add_process_time_header(request: Request, call_next):
    start = time.perf_counter()
    response = await call_next(request)
    response.headers["X-Process-Time"] = f"{time.perf_counter() - start:.6f}"
    return response


@app.get("/users/{user_id}")
async def get_user(user_id: int) -> dict[str, int]:
    return {"user_id": user_id}
```

## 3. When to use

* Když chcete jednotně řešit cross-cutting concerns (logování, auth, observability).
* Když potřebujete přehledný routing a hierarchii endpointů podle domény.

## 4. Common mistakes

* ❌ Příliš mnoho těžké logiky v middleware – zpomalí každý request.
* ❌ Nekonzistentní routing pravidla napříč moduly – API působí chaoticky.

---

<details>
<summary>Optional: Deep dive</summary>

U větších API se osvědčuje modulární routing (např. router pro users, orders, billing). Middleware by měl být malý, rychlý a dobře měřitelný.

</details>

---

**Další krok:** [Authentication](Authentication.md)
