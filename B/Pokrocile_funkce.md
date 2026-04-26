# Pokročilé funkce

V této kapitole si projdeme `lambda`, closures a decorators, tedy techniky pro flexibilnější práci s funkcemi.

## 1. Concept

Funkce jsou v Pythonu objekty první třídy, takže je můžete předávat jako argumenty nebo vracet z jiných funkcí.

- `lambda` je krátká anonymní funkce pro jednoduché výrazy.
- closure je funkce, která si pamatuje proměnné z okolního scope.
- decorator je funkce, která "obalí" jinou funkci a přidá chování (např. logování, měření času, autorizaci).

Tyto nástroje existují hlavně proto, abyste mohli znovupoužívat logiku bez kopírování kódu.

## 2. Example

```python
from collections.abc import Callable

# lambda
items = [("jablko", 25), ("hruska", 30), ("banan", 20)]
sorted_items = sorted(items, key=lambda item: item[1])

# closure
def make_multiplier(factor: int) -> Callable[[int], int]:
    def multiply(value: int) -> int:
        return value * factor
    return multiply

# decorator
def log_call(func: Callable[..., object]) -> Callable[..., object]:
    def wrapper(*args: object, **kwargs: object) -> object:
        print(f"Volam {func.__name__} s args={args}, kwargs={kwargs}")
        return func(*args, **kwargs)
    return wrapper

@log_call
def add(a: int, b: int) -> int:
    return a + b

triple = make_multiplier(3)
print(sorted_items)
print(triple(7))
print(add(2, 5))
```

## 3. When to use

* Když potřebujete parametrizovat chování (closures) bez vytváření celé třídy.
* Když chcete oddělit průřezovou logiku (dekorátory), například audit nebo měření času.

## 4. Common mistakes

* ❌ Použití `lambda` pro složitou logiku – lépe funguje běžná `def` funkce s názvem.
* ❌ Zapomenutí vrátit výsledek ve wrapperu dekorátoru – obalená funkce pak "ztratí" návratovou hodnotu.

---

<details>
<summary>Optional: Deep dive</summary>

U dekorátorů v produkci často použijete `functools.wraps`, aby se zachovaly metadata původní funkce (název, docstring). To pomáhá při debugování i v dokumentaci.

</details>

---

**Další krok:** [Generátory a iterátory](Generatory_a_iteratory.md)
