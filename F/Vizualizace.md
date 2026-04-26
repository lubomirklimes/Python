# Vizualizace

Tato kapitola ukazuje základy datové vizualizace pomocí matplotlib a seaborn.

## 1. Concept

Vizualizace pomáhá rychle odhalit trendy, odlehlé hodnoty a vztahy v datech. `matplotlib` je univerzální základ, zatímco `seaborn` staví nad matplotlib a nabízí přehlednější API pro statistické grafy.

Praktický přístup: začít jednoduchým grafem, ověřit čitelnost os a legendy, potom případně přidávat detaily.

## 2. Example

```python
import matplotlib.pyplot as plt
import seaborn as sns
import pandas as pd

sns.set_theme(style="whitegrid")

df = pd.DataFrame(
    {
        "month": ["Jan", "Feb", "Mar", "Apr"],
        "sales": [120, 150, 130, 170],
    }
)

plt.figure(figsize=(7, 4))
sns.lineplot(data=df, x="month", y="sales", marker="o")
plt.title("Měsíční prodeje")
plt.xlabel("Měsíc")
plt.ylabel("Prodeje")
plt.tight_layout()
plt.show()
```

## 3. When to use

* Když chcete ověřit hypotézu nad daty před dalším modelováním.
* Když připravujete report nebo prezentaci pro tým či stakeholdery.

## 4. Common mistakes

* ❌ Přeplněný graf s příliš mnoha sériemi - informace se ztratí.
* ❌ Chybějící popisky os a jednotek - graf je pak těžké správně interpretovat.

---

<details>
<summary>Optional: Deep dive</summary>

U produkčních dashboardů řešte konzistentní škály, barvy a přístupnost. Pěkný graf není cíl, cíl je srozumitelná interpretace dat bez zavádějících vizuálních efektů.

</details>

---

**Další krok:** [Machine Learning](Machine_Learning.md)
