# Čištění dat a chybějící hodnoty

Tato kapitola řeší praktické čištění dat, chybějící hodnoty a konzistenci typů.

## 1. Concept

Reálná data bývají neúplná, nejednotná a obsahují chyby. Čištění dat znamená odstranit nebo opravit nekonzistence tak, aby další analýza nebyla zavádějící.

Typické kroky:

- sjednotit datové typy
- ošetřit chybějící hodnoty
- odstranit duplicity
- validovat rozsahy a doménová pravidla

## 2. Example

```python
import pandas as pd

raw = pd.DataFrame(
    {
        "user_id": [1, 2, 2, 3],
        "age": [25, None, None, -1],
        "email": ["a@x.cz", "b@x.cz", "b@x.cz", None],
    }
)

df = raw.drop_duplicates().copy()
df["age"] = df["age"].fillna(df["age"].median())
df.loc[df["age"] < 0, "age"] = pd.NA
df["age"] = df["age"].fillna(df["age"].median())

df["email"] = df["email"].fillna("unknown@example.com")

df["age"] = df["age"].astype("int64")
print(df)
```

## 3. When to use

* Když získáváte data z více zdrojů s různou kvalitou.
* Když chcete stabilní vstupy pro reporting nebo ML pipeline.
* Když připravujete data na automatizované zpracování.

## 4. Common mistakes

* ❌ Nahrazení chybějících hodnot bez doménového zdůvodnění.
* ❌ Odstranění "problematických" řádků bez auditu dopadu.
* ❌ Smíchání čistění train a test dat nevhodným způsobem.

---

<details>
<summary>Optional: Deep dive</summary>

Strategie imputace závisí na typu dat a cíli projektu. U některých modelů je lepší přidat i indikátor, že původní hodnota chyběla.

</details>

---

**Další krok:** [Feature engineering](Feature_engineering.md)
