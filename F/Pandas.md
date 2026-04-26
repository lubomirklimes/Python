# Pandas

Tato kapitola vysvětluje práci s tabulkovými daty přes Pandas a základní datové transformace.

## 1. Concept

Pandas je knihovna pro práci s daty ve formě tabulek (`DataFrame`) a sloupců (`Series`). Umožňuje rychle načítat CSV/Excel, čistit data, filtrovat řádky, agregovat hodnoty a připravit dataset pro analýzu nebo modelování.

Běžný workflow: načtení dat -> kontrola kvality -> transformace a agregace -> export výsledku nebo předání do vizualizace/ML.

## 2. Example

```python
import pandas as pd

sales = pd.DataFrame(
    {
        "city": ["Praha", "Brno", "Praha", "Ostrava"],
        "amount": [1200, 800, 950, 400],
        "paid": [True, False, True, True],
    }
)

paid_sales = sales[sales["paid"]]
summary = paid_sales.groupby("city", as_index=False)["amount"].sum()
summary = summary.sort_values("amount", ascending=False)

print(summary)
```

## 3. When to use

* Když potřebujete rychle analyzovat a čistit tabulková data.
* Když připravujete vstupní dataset pro reporting nebo machine learning.

## 4. Common mistakes

* ❌ Řetězení mnoha operací bez průběžné kontroly dat - chyby se pak hledají těžko.
* ❌ Nejednoznačná práce s chybějícími hodnotami (`NaN`) - výsledné statistiky mohou být zkreslené.

---

<details>
<summary>Optional: Deep dive</summary>

Pandas je skvělé pro malé a střední datasety v paměti. Pokud data přerostou RAM, zvažte nástroje jako Polars, Dask nebo SQL engine, případně pipeline po dávkách.

</details>

---

**Další krok:** [Vizualizace](Vizualizace.md)
