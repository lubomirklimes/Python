# Řízení toku programu

Tato kapitola ukazuje, jak rozhodovat podle podmínek a jak opakovat kód pomocí smyček.

## 1. Concept

Řízení toku říká programu, jakou cestou pokračovat. Pomocí `if`, `elif`, `else` vybíráte větev podle podmínky. Smyčka `for` je vhodná pro průchod kolekcí, `while` pro opakování, dokud platí podmínka.

Speciální příkazy:

- `break` okamžitě ukončí nejbližší smyčku
- `continue` přeskočí zbytek aktuální iterace
- `pass` nedělá nic (hodí se jako dočasné místo v kódu)

## 2. Example

```python
numbers = [3, 7, 0, 9, 12]

for n in numbers:
    if n == 0:
        continue
    if n > 10:
        print("Nasel jsem cislo vetsi nez 10, koncim smycku.")
        break
    print(f"Zpracovavam: {n}")

attempts = 3
while attempts > 0:
    print(f"Zbyvajici pokusy: {attempts}")
    attempts -= 1

if attempts == 0:
    pass
```

## 3. When to use

* Když chcete reagovat na různé stavy programu (např. validní/nevalidní vstup).
* Když potřebujete opakovaně zpracovat seznam dat nebo čekat na splnění podmínky.

## 4. Common mistakes

* ❌ Zapomenuté snižování počítadla ve `while` – vznikne nekonečná smyčka.
* ❌ Použití `break` na špatném místě – program skončí dřív, než má.

---

<details>
<summary>Optional: Deep dive</summary>

`for` v Pythonu iteruje přes iterovatelné objekty, nejen přes číselné rozsahy. Proto je idiomatické používat `for item in data` místo manuálního indexování, pokud index opravdu nepotřebujete.

</details>

---

**Další krok:** [Funkce](Funkce.md)
