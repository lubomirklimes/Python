# Data pipeline - načtení, čištění a transformace dat

Tato kapitola popisuje praktický pipeline mindset od vstupu dat až po připravený výstup pro model.

## 1. Concept

Data pipeline je opakovatelný postup:

input -> validation -> cleaning -> transformation -> feature engineering -> split -> output

Proč je to důležité: kvalita modelu je přímo závislá na kvalitě dat. Pokud pipeline nehlídá validaci a konzistenci, model se učí šum a chyby.

Klíčové kroky:

- validace: povinné sloupce, typy, rozsahy hodnot
- čištění: chybějící hodnoty, duplicity, neplatné řádky
- transformace: normalizace, kódování kategorií
- split: train/validation/test bez leakage

## 2. Example

```python
import pandas as pd
from sklearn.model_selection import train_test_split

# input
raw = pd.DataFrame(
    {
        "age": [25, 31, 40, None, 28, 35],
        "income": [30000, 42000, 58000, 61000, None, 49000],
        "target": [0, 0, 1, 1, 0, 1],
    }
)

# validation
required = {"age", "income", "target"}
missing_cols = required - set(raw.columns)
if missing_cols:
    raise ValueError(f"Chybí sloupce: {missing_cols}")

# cleaning
df = raw.drop_duplicates().copy()
df["age"] = df["age"].fillna(df["age"].median())
df["income"] = df["income"].fillna(df["income"].median())

# transformation + feature engineering
df["income_per_age"] = df["income"] / df["age"]

X = df[["age", "income", "income_per_age"]]
y = df["target"]

# split train / validation / test
X_temp, X_test, y_temp, y_test = train_test_split(X, y, test_size=0.2, random_state=42, stratify=y)
X_train, X_val, y_train, y_val = train_test_split(X_temp, y_temp, test_size=0.25, random_state=42, stratify=y_temp)

print("train:", len(X_train), "val:", len(X_val), "test:", len(X_test))
```

## 3. When to use

* Když připravujete data pro trénink modelu nebo pravidelný scoring.
* Když chcete mít auditovatelný a reprodukovatelný datový workflow.
* Když potřebujete oddělit experimentální a produkční transformace.

## 4. Common mistakes

* ❌ Míchání train a test dat při čištění nebo scalingu (data leakage).
* ❌ Ruční ad-hoc úpravy dat bez uloženého pipeline kroku.
* ❌ Přeskočení validace vstupu, i když data přichází z více zdrojů.

---

<details>
<summary>Optional: Deep dive</summary>

Pipeline by měla být deterministická a verzovaná. V praxi se vyplatí ukládat i metadata běhu: verzi vstupních dat, parametry transformace a hash modelu, který byl na datech trénován.

</details>

---

**Další krok:** [Datové formáty a ukládání dat](Datove_formaty.md)
