# Kvalita dat a validace

Tato kapitola shrnuje, jak kontrolovat kvalitu dat a proč je validace klíčová pro spolehlivé výsledky.

## 1. Concept

Kvalita dat rozhoduje o kvalitě analýzy i modelu. Validace znamená ověřit, že data splňují očekávaný kontrakt: správné sloupce, typy, rozsahy, unikátnost a logické vztahy.

Typické validační pravidla:

- schema validation (sloupce + datové typy)
- business rules (např. věk >= 0)
- missing values thresholds
- uniqueness constraints

## 2. Example

```python
import pandas as pd

df = pd.DataFrame(
    {
        "user_id": [1, 2, 2],
        "age": [25, 30, -4],
        "email": ["a@x.cz", "b@x.cz", "b@x.cz"],
    }
)

errors = []
if not df["user_id"].is_unique:
    errors.append("user_id není unikátní")
if (df["age"] < 0).any():
    errors.append("nalezen záporný věk")
if df["email"].isna().any():
    errors.append("chybí email")

print("VALID" if not errors else f"INVALID: {errors}")
```

## 3. When to use

* Když ingestujete data z externích systémů.
* Když provozujete pravidelnou datovou pipeline.
* Když chcete snížit riziko tichých chyb v reportingu a ML.

## 4. Common mistakes

* ❌ Validace jen při vývoji, ale ne v produkční pipeline.
* ❌ Chybějící alerting při porušení pravidel.
* ❌ Příliš volná pravidla, která propustí nekvalitní data dál.

---

<details>
<summary>Optional: Deep dive</summary>

Data quality checks by měly být součást CI/CD pro data pipeline. Užitečné je sledovat trend kvality v čase, ne jen jednorázový stav.

</details>

---

**Další krok:** [Zpět na obsah](../README.md)
