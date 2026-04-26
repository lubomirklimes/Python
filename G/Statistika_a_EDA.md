# Statistika a EDA

Tato kapitola ukazuje základní statistické myšlení a EDA (exploratory data analysis) před modelováním.

## 1. Concept

EDA je první praktický krok po načtení dat. Cílem je pochopit distribuce, odlehlé hodnoty, vztahy mezi proměnnými a kvalitu dat. Statistiky jako průměr, medián, kvantily nebo směrodatná odchylka pomáhají odhalit, jestli data dávají smysl.

Důležité je kombinovat numerický souhrn a vizuální kontrolu. Samotný průměr často nestačí, protože neukáže tvar distribuce ani extrémní hodnoty.

## 2. Example

```python
import pandas as pd

sales = pd.DataFrame(
    {
        "amount": [100, 120, 130, 5000, 110, 125],
        "items": [1, 2, 2, 20, 1, 2],
    }
)

print(sales.describe())
print("Medián amount:", sales["amount"].median())
print("Q1/Q3:", sales["amount"].quantile([0.25, 0.75]).to_dict())
print("Korelace:\n", sales.corr(numeric_only=True))
```

## 3. When to use

* Když dostanete nový dataset a potřebujete rychle pochopit jeho charakter.
* Když připravujete datovou validaci před pipeline nebo reportem.
* Když hledáte odlehlé body a možné datové chyby.

## 4. Common mistakes

* ❌ Přeskočení EDA a rovnou trénink modelu.
* ❌ Interpretace průměru bez kontextu distribuce.
* ❌ Ignorování outlierů, které mohou ovlivnit model i byznys závěr.

---

<details>
<summary>Optional: Deep dive</summary>

V EDA je užitečné porovnávat segmenty dat (např. region, produkt, období), ne jen celek. Problémy se často skrývají v konkrétní podmnožině.

</details>

---

**Další krok:** [Čištění dat a chybějící hodnoty](Cisteni_dat.md)
