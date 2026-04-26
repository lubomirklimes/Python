# Pandas - práce s daty

Tato kapitola ukazuje práci s tabulkovými daty přes `DataFrame` a `Series` a přípravu dat pro machine learning.

## 1. Concept

Pandas nabízí dvě klíčové struktury:

- `Series`: jednorozměrný sloupec hodnot
- `DataFrame`: tabulka složená ze sloupců

V praxi řešíte hlavně načtení dat (CSV/Parquet), filtrování, agregace, práci s chybějícími hodnotami a tvorbu feature sloupců. Cílem je dostat data do konzistentního stavu pro analýzu nebo model.

## 2. Example

```python
import pandas as pd
from io import StringIO

csv_data = """city,age,income
Praha,29,50000
Brno,,42000
Praha,35,60000
Ostrava,41,
"""

# načtení CSV
df = pd.read_csv(StringIO(csv_data))

# práce s chybějícími hodnotami
df["age"] = df["age"].fillna(df["age"].median())
df["income"] = df["income"].fillna(df["income"].mean())

# filtrování + agregace
filtered = df[df["income"] > 45000]
summary = filtered.groupby("city", as_index=False).agg(avg_income=("income", "mean"))

# jednoduchá feature pro ML
df["income_per_age"] = df["income"] / df["age"]

print(df)
print(summary)
```

## 3. When to use

* Když čistíte surová tabulková data před analýzou nebo modelováním.
* Když potřebujete rychlé agregace a filtrování bez psaní SQL.
* Když připravujete vstupní features pro ML pipeline.

## 4. Common mistakes

* ❌ Ignorování `NaN` hodnot a následná práce s nekompletními daty.
* ❌ Přepisování dat bez auditovatelného pipeline kroku.
* ❌ Chybné datové typy sloupců (např. čísla jako string), které zkreslí výpočty.

---

<details>
<summary>Optional: Deep dive</summary>

Při přípravě dat pro ML je důležitá konzistence transformací mezi tréninkem a inferencí. Co uděláte na tréninkových datech (imputace, scaling), musíte stejně aplikovat na nová data v produkci.

</details>

---

**Další krok:** [Vizualizace dat](Vizualizace.md)
