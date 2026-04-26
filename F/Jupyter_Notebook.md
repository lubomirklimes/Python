# Jupyter Notebook

Tato kapitola ukazuje, jak využít Jupyter Notebook pro interaktivní vývoj a datové experimenty.

## 1. Concept

Jupyter Notebook kombinuje kód, text a výstupy v jednom dokumentu. Je ideální pro exploraci dat, prototypování modelů a sdílení analýz. Umožňuje spouštět buňky po částech a rychle iterovat nad nápady.

Praktický workflow: načíst data, dělat malé ověřitelné kroky, průběžně dokumentovat rozhodnutí a na závěr převést stabilní část do běžného Python modulu.

## 2. Example

```python
import pandas as pd

# V notebooku by tento kód byl v samostatné buňce.
df = pd.DataFrame({"x": [1, 2, 3], "y": [2, 4, 8]})
df["ratio"] = df["y"] / df["x"]
print(df)
```

## 3. When to use

* Když analyzujete data a potřebujete rychle testovat hypotézy.
* Když tvoříte edukativní materiály, reporty nebo reprodukovatelné experimenty.

## 4. Common mistakes

* ❌ Spoléhání na skrytý stav kernelu - notebook pak nejde spustit od začátku bez chyb.
* ❌ Ponechání produkční logiky jen v notebooku bez převodu do testovatelných modulů.

---

<details>
<summary>Optional: Deep dive</summary>

Dobrá praxe je pravidelně spouštět notebook "Run all" od první buňky a mít jasné závislosti. Tím snížíte riziko nereprodukovatelných výsledků.

</details>

---

**Další krok:** [Data pipelines](Data_pipelines.md)
