# Typování v Pythonu

V této kapitole se naučíte základy type hints, práci s modulem `typing` a kontrolu typů pomocí nástroje mypy.

## 1. Concept

Type hints jsou anotace typů, které zlepšují čitelnost a umožňují statickou kontrolu chyb ještě před spuštěním programu. Python je stále dynamický jazyk, ale typové anotace pomáhají editoru i nástrojům odhalit problémy dříve.

Modul `typing` nabízí užitečné typy jako `Any`, `Optional`, `TypedDict`, `Protocol` nebo `Callable`. Pro reálné projekty je běžné spouštět `mypy`, které ověří konzistenci anotací.

## 2. Example

```python
from typing import TypedDict

class User(TypedDict):
    name: str
    age: int


def format_user(user: User) -> str:
    return f"{user['name']} ({user['age']})"


def average(values: list[float]) -> float:
    if not values:
        raise ValueError("Seznam nesmi byt prazdny")
    return sum(values) / len(values)

u: User = {"name": "Eva", "age": 29}
print(format_user(u))
print(average([1.5, 2.5, 3.0]))
```

Kontrola typů v praxi:

```bash
mypy .
```

## 3. When to use

* Když projekt roste a potřebujete rychleji zachytit typové chyby při refaktoringu.
* Když na kódu pracuje více lidí a chcete mít jasné kontrakty funkcí.

## 4. Common mistakes

* ❌ Používání `Any` všude – tím se ztrácí přínos typové kontroly.
* ❌ Mylná představa, že type hints mění runtime chování Pythonu – samy o sobě nic nevynucují.

---

<details>
<summary>Optional: Deep dive</summary>

V Pythonu 3.11 je běžné používat moderní zápis jako `list[str]` nebo `dict[str, int]` místo staršího `List[str]`. Statická typová kontrola je nejefektivnější, když ji zapojíte do CI pipeline.

</details>

---

**Další krok:** [Výjimky a error handling](Vyjimky_a_error_handling.md)
