# Kolekce

Tato kapitola shrnuje čtyři základní kolekce v Pythonu: `list`, `tuple`, `set` a `dict`.

## 1. Concept

Kolekce slouží k ukládání více hodnot. Každý typ má jiné vlastnosti:

- `list`: uspořádaný a měnitelný seznam
- `tuple`: uspořádaná a neměnitelná n-tice
- `set`: neuspořádaná množina unikátních hodnot
- `dict`: mapování klíč -> hodnota

Volba správné kolekce zjednoduší kód a často i zlepší výkon.

## 2. Example

```python
fruits = ["jablko", "hruska", "jablko"]
point = (10, 20)
unique_fruits = set(fruits)
prices = {"jablko": 25, "hruska": 30}

fruits.append("banan")

print(f"List: {fruits}")
print(f"Tuple: {point}")
print(f"Set: {unique_fruits}")
print(f"Dict cena jablka: {prices['jablko']}")
```

## 3. When to use

* Když potřebujete uchovat pořadí a případné duplicity (`list`).
* Když chcete rychle vyhledávat podle klíče (`dict`) nebo odstranit duplicity (`set`).

## 4. Common mistakes

* ❌ Očekávání pevného pořadí u `set` – množina pořadí negarantuje.
* ❌ Přístup k neexistujícímu klíči ve `dict` bez ošetření – může vzniknout `KeyError`.

---

<details>
<summary>Optional: Deep dive</summary>

`tuple` se často používá pro hodnoty, které se nemají měnit (např. souřadnice). `dict` od Pythonu 3.7 zachovává pořadí vložení, ale hlavní důvod pro použití slovníku je stále rychlé mapování klíčů na hodnoty.

</details>

---

**Další krok:** [Práce se stringy](Prace_se_stringy.md)
