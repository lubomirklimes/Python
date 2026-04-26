# Vizualizace dat

Tato kapitola vysvětluje, jak používat matplotlib a seaborn pro pochopení dat a odhalení chyb.

## 1. Concept

Vizualizace je rychlý nástroj na ověření, jestli data dávají smysl. Než začnete trénovat model, je dobré pochopit rozdělení hodnot, odlehlé body a vztahy mezi proměnnými.

Běžné grafy:

- histogram: rozdělení jedné proměnné
- scatter plot: vztah dvou proměnných
- line chart: vývoj v čase

`matplotlib` dává nízkoúrovňovou kontrolu, `seaborn` přidává pohodlné API a hezčí výchozí styly.

## 2. Example

```python
import matplotlib.pyplot as plt
import seaborn as sns
import pandas as pd

sns.set_theme(style="whitegrid")

df = pd.DataFrame(
    {
        "step": [1, 2, 3, 4, 5, 6],
        "x": [1, 2, 3, 4, 5, 6],
        "y": [1.1, 1.9, 3.2, 3.8, 5.1, 8.7],  # poslední bod vypadá jako outlier
        "value": [5, 7, 6, 8, 5, 9],
    }
)

fig, axes = plt.subplots(1, 3, figsize=(14, 4))

sns.histplot(df["value"], bins=5, kde=True, ax=axes[0])
axes[0].set_title("Histogram")

sns.scatterplot(data=df, x="x", y="y", ax=axes[1])
axes[1].set_title("Scatter")

sns.lineplot(data=df, x="step", y="y", marker="o", ax=axes[2])
axes[2].set_title("Line chart")

plt.tight_layout()
plt.show()
```

## 3. When to use

* Když chcete před modelováním rychle odhalit nekvalitní nebo podezřelá data.
* Když vysvětlujete datové závěry týmu nebo stakeholderům.
* Když porovnáváte vývoj metrik v čase.

## 4. Common mistakes

* ❌ Graf bez popisků os a jednotek - výsledky jsou neinterpretovatelné.
* ❌ Přesycené vizualizace s příliš mnoha sériemi najednou.
* ❌ Spoléhání pouze na agregované metriky bez vizuální kontroly distribuce.

---

<details>
<summary>Optional: Deep dive</summary>

Vizualizace pomáhá odhalit chyby, které metriky skryjí, například leakage, outliery nebo posun distribuce mezi train a test daty. U ML projektů je to jedna z nejlevnějších a nejúčinnějších kontrol kvality.

</details>

---

**Další krok:** [Statistika a EDA](Statistika_a_EDA.md)
