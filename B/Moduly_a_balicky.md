# Moduly a balíčky

V této kapitole si ukážeme, jak organizovat kód do modulů a balíčků a jak správně používat importy.

## 1. Concept

Modul je jeden `.py` soubor. Balíček je složka s více moduly, typicky obsahuje i soubor `__init__.py`. Rozdělení projektu do modulů zlepšuje přehlednost a testovatelnost.

Importy by měly být čitelné a stabilní. Obvykle je dobré držet jasnou strukturu projektu, nepoužívat chaotické kruhové importy a exportovat veřejné API balíčku přes `__init__.py`.

## 2. Example

```python
# shop/pricing.py
def total_price(items: list[float], vat: float = 0.21) -> float:
    return sum(items) * (1 + vat)

# shop/__init__.py
from .pricing import total_price

# main.py
from shop import total_price

print(total_price([100.0, 250.0]))
```

## 3. When to use

* Když projekt přeroste jeden soubor a potřebujete oddělit zodpovědnosti.
* Když chcete vytvořit znovupoužitelnou knihovnu s jasným veřejným API.

## 4. Common mistakes

* ❌ Náhodné importy napříč projektem bez struktury – vzniká těžko udržovatelný "spaghetti" kód.
* ❌ Cyklické importy (A importuje B a B importuje A) – program může padat už při startu.

---

<details>
<summary>Optional: Deep dive</summary>

`__init__.py` může být prázdný, ale často se používá pro export vybraných symbolů balíčku. V moderním Pythonu se někdy setkáte i s namespace packages bez `__init__.py`, ale pro začátek je klasický balíček jednodušší a čitelnější.

</details>

---

**Další krok:** [Práce se soubory](Prace_se_soubory.md)
