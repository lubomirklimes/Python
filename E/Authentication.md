# Authentication

Tato kapitola porovnává session-based autentizaci a JWT tokeny.

## 1. Concept

Autentizace ověřuje, kdo je uživatel. Ve webu se běžně používají dva přístupy:

- session: server drží stav přihlášení (typicky cookie + server-side session storage)
- JWT: klient posílá podepsaný token v `Authorization` hlavičce

Session je jednoduchá pro klasické weby. JWT je časté u API a mikroservis, ale vyžaduje pečlivou práci s expirací, revokací a zabezpečeným ukládáním tokenu.

## 2. Example

```python
from datetime import datetime, timedelta, timezone
import jwt

SECRET_KEY = "change-me-in-production"
ALGORITHM = "HS256"


def create_access_token(user_id: int) -> str:
    payload = {
        "sub": str(user_id),
        "exp": datetime.now(timezone.utc) + timedelta(minutes=30),
    }
    return jwt.encode(payload, SECRET_KEY, algorithm=ALGORITHM)


def decode_access_token(token: str) -> dict:
    return jwt.decode(token, SECRET_KEY, algorithms=[ALGORITHM])


token = create_access_token(42)
print(token)
print(decode_access_token(token)["sub"])
```

## 3. When to use

* Když stavíte web s přihlášením uživatelů a potřebujete chránit soukromé endpointy.
* Když řešíte API přístup mezi klientem a backendem (mobil, SPA, třetí strany).

## 4. Common mistakes

* ❌ Ukládání JWT do nechráněného úložiště bez ochrany proti XSS/CSRF.
* ❌ Chybějící expirace a rotace klíčů – tokeny pak představují dlouhodobé riziko.

---

<details>
<summary>Optional: Deep dive</summary>

Session i JWT mají své místo. Nejde o to, co je "modernější", ale co lépe odpovídá architektuře. U veřejných API se hodí krátká expirace access tokenu a zvlášť řízený refresh token.

</details>

---

**Další krok:** [WebSockets](WebSockets.md)
