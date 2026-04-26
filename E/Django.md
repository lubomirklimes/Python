# Django

Tato kapitola představuje Django jako full-stack framework pro webové aplikace.

## 1. Concept

Django poskytuje kompletní základ: ORM, autentizaci, admin, routing, formuláře, middleware i bezpečnostní ochrany. Je vhodné pro projekty, kde chcete rychle stavět robustní aplikaci s jasnými konvencemi.

Oproti Flasku má Django víc vestavěných částí, takže dává větší produktivitu u standardních scénářů (uživatelé, administrace, modely, migrace).

## 2. Example

```python
# views.py
from django.http import JsonResponse
from django.views.decorators.http import require_GET


@require_GET
def health(request):
    return JsonResponse({"status": "ok"})


# urls.py
from django.urls import path
from .views import health

urlpatterns = [
    path("health/", health, name="health"),
]
```

## 3. When to use

* Když stavíte větší webovou aplikaci s uživateli, databází a admin rozhraním.
* Když chcete framework s dlouhodobě ověřenými konvencemi a silnou komunitou.

## 4. Common mistakes

* ❌ Obcházení Django konvencí bez důvodu – projekt pak ztrácí čitelnost.
* ❌ Slabé rozdělení aplikací (apps) – kód rychle přeroste do neudržitelné struktury.

---

<details>
<summary>Optional: Deep dive</summary>

Django má výborné bezpečnostní základy (CSRF, ochrana proti XSS, ORM proti SQL injection při správném použití). Vyplatí se je využít místo vlastních improvizací.

</details>

---

**Další krok:** [FastAPI](FastAPI.md)
