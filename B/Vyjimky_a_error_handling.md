# Výjimky a error handling

Tato kapitola ukazuje, jak bezpečně zachytávat chyby pomocí `try/except` a jak navrhnout vlastní výjimky.

## 1. Concept

Výjimky slouží k řízení chybových stavů. Místo vracení "speciálních hodnot" (např. `None` pro chybu) můžete vyvolat výjimku, která nese jasnou informaci o problému.

`try/except` používejte co nejkonkrétněji: zachytávejte jen ty chyby, které umíte rozumně vyřešit. Pro doménovou logiku se hodí vlastní výjimky (custom exceptions), které lépe popisují byznys chybu.

## 2. Example

```python
class InvalidAgeError(ValueError):
    """Věk musí být nezáporné celé číslo."""


def parse_age(value: str) -> int:
    try:
        age = int(value)
    except ValueError as exc:
        raise InvalidAgeError("Věk musí být číslo") from exc

    if age < 0:
        raise InvalidAgeError("Věk nesmí být záporný")

    return age


for raw in ["22", "-5", "abc"]:
    try:
        print(f"Validni vek: {parse_age(raw)}")
    except InvalidAgeError as err:
        print(f"Chyba vstupu: {err}")
```

## 3. When to use

* Když chcete oddělit hlavní tok programu od chybových scénářů.
* Když potřebujete vracet přesnější chyby z vlastní doménové logiky.

## 4. Common mistakes

* ❌ `except Exception:` bez dalšího důvodu – schová i chyby, které byste měli opravit.
* ❌ Polykání výjimky bez logu nebo reakce – problém se pak těžko dohledává.

---

<details>
<summary>Optional: Deep dive</summary>

Konstrukce `raise NewError(...) from exc` zachová původní příčinu chyby (exception chaining). Díky tomu je traceback výrazně užitečnější při debugování.

</details>

---

**Další krok:** [Moduly a balíčky](Moduly_a_balicky.md)
