# Práce se soubory

Tato kapitola vysvětluje bezpečné čtení a zápis souborů a proč je v Pythonu důležitý context manager `with`.

## 1. Concept

Při práci se soubory je klíčové správně je otevřít a zavřít. Konstrukce `with open(...) as f:` zajistí, že se soubor uzavře automaticky i při chybě. To je bezpečnější než ruční `open()` a `close()`.

V praxi téměř vždy nastavujte `encoding="utf-8"`, abyste předešli problémům s českými znaky. Pro textové soubory používejte režim `r` (read), `w` (write) nebo `a` (append).

## 2. Example

```python
from pathlib import Path

path = Path("notes.txt")

# zápis
with path.open("w", encoding="utf-8") as file:
    file.write("Prvni radek\n")
    file.write("Druhy radek\n")

# čtení
with path.open("r", encoding="utf-8") as file:
    content = file.read()

print(content)
```

## 3. When to use

* Když ukládáte konfiguraci, logy nebo exportované výsledky do textových souborů.
* Když načítáte vstupní data ze souboru a chcete mít jistotu správného uzavření zdrojů.

## 4. Common mistakes

* ❌ Otevření souboru bez `with` a zapomenuté `close()` – hrozí únik systémových prostředků.
* ❌ Neurčené kódování – na jiném systému se mohou špatně zobrazit znaky s diakritikou.

---

<details>
<summary>Optional: Deep dive</summary>

Context managery fungují nejen pro soubory, ale i pro síťová spojení, databázové transakce nebo zámky vláken. Princip je stejný: jasně vymezit začátek a bezpečný úklid zdroje.

</details>

---

**Další krok:** [Zpět na obsah](../README.md)
