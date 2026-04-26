# Pythonic code

Tato kapitola ukazuje typické idiomy Pythonu a vysvětluje, jak hledat rovnováhu mezi čitelností a výkonem.

## 1. Concept

"Pythonic" kód využívá styl, který je v Python komunitě považovaný za přirozený: je krátký, čitelný a snadno udržovatelný. Typický idiom je list comprehension, tedy stručný zápis pro vytvoření nového seznamu z iterovatelného objektu.

Důležité pravidlo je: nejdřív čitelnost, potom optimalizace. Ne každý kratší zápis je automaticky lepší. Pokud je výraz příliš složitý, bývá lepší obyčejná smyčka s jasnými kroky.

## 2. Example

```python
numbers = [1, 2, 3, 4, 5, 6]

# Pythonic varianta: filtr + transformace v jednom výrazu
squares_of_even = [n * n for n in numbers if n % 2 == 0]

# Čitelnější varianta pro složitější logiku
result = []
for n in numbers:
    if n % 2 == 0:
        transformed = n * n
        result.append(transformed)

print(squares_of_even)
print(result)
```

## 3. When to use

* Když potřebujete stručně zpracovat kolekci jednoduchou transformací nebo filtrem.
* Když chcete psát konzistentní kód, který ostatní Python vývojáři rychle přečtou.

## 4. Common mistakes

* ❌ Příliš komplikovaná list comprehension s více podmínkami a vedlejší logikou – kód je pak hůř čitelný než klasická smyčka.
* ❌ Předčasná optimalizace bez měření – můžete zhoršit čitelnost, ale výkon se téměř nezmění.

---

<details>
<summary>Optional: Deep dive</summary>

U výkonu platí, že mnoho Pythonic idiomů je rychlých, protože interně využívají optimalizované C implementace. Přesto je lepší měřit konkrétní scénář (např. pomocí `timeit`) než odhadovat výkon "od oka".

</details>

---

**Další krok:** [Pokročilé funkce](Pokrocile_funkce.md)
