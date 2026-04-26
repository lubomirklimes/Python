# Základní syntaxe

V této kapitole si ukážeme proměnné, základní datové typy a práci s funkcemi `print` a `input`.

## 1. Concept

V Pythonu jsou proměnné dynamicky typované. To znamená, že typ neurčujete předem, ale odvodí se podle přiřazené hodnoty. Jedna proměnná může během programu držet různé typy, ale v jednom okamžiku má vždy jen jeden konkrétní typ.

Základní typy, se kterými začnete nejčastěji:

- `int` pro celá čísla
- `float` pro desetinná čísla
- `str` pro text
- `bool` pro `True`/`False`

`print()` slouží k výpisu na obrazovku a `input()` načítá text od uživatele. Důležité je, že `input()` vrací vždy `str`, takže čísla je potřeba převést.

## 2. Example

```python
name = input("Jak se jmenujes? ")
age_text = input("Kolik ti je let? ")
age = int(age_text)

height = 1.82      # float
is_adult = age >= 18  # bool

print(f"Ahoj {name}!")
print(f"Vek: {age}, vyska: {height} m, dospely: {is_adult}")
```

## 3. When to use

* Když sbíráte jednoduchý vstup od uživatele v konzolové aplikaci.
* Když potřebujete rychle ověřit data nebo mezivýsledek přes `print()`.

## 4. Common mistakes

* ❌ Očekávání, že `input()` vrátí číslo – bez `int()` nebo `float()` dostanete text.
* ❌ Přepis proměnné jiným typem bez kontroly – později to může způsobit chybu v logice.

---

<details>
<summary>Optional: Deep dive</summary>

Větší projekty často používají typové anotace (`name: str`, `age: int`) pro lepší čitelnost a kontrolu pomocí nástrojů jako Pylance nebo mypy. Samotný Python je ale při běhu stále dynamický.

</details>

---

**Další krok:** [Řízení toku programu](Rizeni_toku_programu.md)
