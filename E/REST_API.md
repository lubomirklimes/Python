# REST API

Tato kapitola popisuje návrh REST API, konzistentní endpointy a validaci dat.

## 1. Concept

Dobré REST API je předvídatelné: používá smysluplné názvy zdrojů, správné HTTP metody a konzistentní strukturu odpovědí. Validace vstupů je zásadní, protože API je veřejný kontrakt mezi klientem a serverem.

Praktické zásady:

- zdroje pojmenovávejte v množném čísle (`/users`, `/orders`)
- používejte stavové kódy (`200`, `201`, `400`, `404`, `409`, `500`)
- vracejte jasnou chybu s detailní zprávou

## 2. Example

```python
from pydantic import BaseModel, Field


class CreateUserRequest(BaseModel):
    email: str = Field(min_length=5)
    age: int = Field(ge=0)


class UserResponse(BaseModel):
    id: int
    email: str
    age: int


def create_user(payload: dict) -> dict:
    req = CreateUserRequest.model_validate(payload)
    user = UserResponse(id=1, email=req.email, age=req.age)
    return user.model_dump()


print(create_user({"email": "eva@example.com", "age": 30}))
```

## 3. When to use

* Když navrhujete API, které budou používat frontendy, mobilní aplikace nebo externí integrace.
* Když potřebujete jasný a stabilní kontrakt mezi službami.

## 4. Common mistakes

* ❌ Nejednotné názvy endpointů a různá struktura chyb – klienti mají složitější implementaci.
* ❌ Slabá validace vstupu – data v systému ztrácí kvalitu a hůř se opravují.

---

<details>
<summary>Optional: Deep dive</summary>

U veřejných API je důležitá verzace (`/v1/...`) a backward compatibility. Při změnách kontraktu je lepší přidat novou verzi než rozbít existující klienty.

</details>

---

**Další krok:** [Middleware a routing](Middleware_a_routing.md)
