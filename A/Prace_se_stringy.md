# Práce se stringy

V této kapitole si ukážeme výřezy textu (slicing) a moderní formátování pomocí f-string.

## 1. Concept

`str` je v Pythonu neměnitelný datový typ. Nový text proto vzniká vytvořením nové hodnoty, ne úpravou původního znaku „na místě“. Slicing (`text[start:stop:step]`) umožňuje vybírat části řetězce.

F-string (`f"..."`) je nejčitelnější způsob skládání textu a proměnných. Umožňuje i formátování čísel, zarovnání nebo počet desetinných míst.

## 2. Example

```python
full_name = "Ada Lovelace"
first_name = full_name[:3]
last_name = full_name[4:]
reversed_name = full_name[::-1]

price = 1234.567
message = f"Uzivatel: {first_name}... {last_name}, cena: {price:.2f} Kč"

print(first_name)
print(last_name)
print(reversed_name)
print(message)
```

## 3. When to use

* Když potřebujete z textu vybrat část (např. prefix, příponu, doménu).
* Když skládáte uživatelské zprávy, logy nebo reporty s proměnnými.

## 4. Common mistakes

* ❌ Špatné indexy ve slicingu – výsledkem je jiná část textu, než očekáváte.
* ❌ Starší a nepřehledné skládání přes `+` místo čitelnějších f-stringů.

---

<details>
<summary>Optional: Deep dive</summary>

U f-stringů můžete použít i pokročilejší formátování, například `f"{value:>10.2f}"` pro zarovnání doprava na šířku 10. Při práci s uživatelským vstupem ale vždy myslete na validaci, protože text nemusí mít očekávaný formát.

</details>

---

**Další krok:** [Zpět na obsah](../README.md)
