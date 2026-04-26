# NumPy

Tato kapitola ukazuje základy práce s poli v NumPy a rychlé vektorové operace.

## 1. Concept

NumPy je základní knihovna pro numerické výpočty v Pythonu. Hlavní objekt je `ndarray`, tedy homogenní pole hodnot stejného typu. Oproti běžnému seznamu v Pythonu je NumPy výrazně rychlejší pro hromadné operace, protože výpočty probíhají ve vektorizované podobě.

Typický workflow je: načíst data, převést je do `ndarray`, provést matematické operace nad celým polem a předat výsledek dál (například do Pandas, vizualizace nebo ML modelu).

## 2. Example

```python
import numpy as np

values = np.array([1, 2, 3, 4, 5], dtype=np.float64)
scaled = values * 1.5
centered = scaled - scaled.mean()

matrix = np.array([[1, 2], [3, 4]], dtype=np.int64)
transposed = matrix.T

print("scaled:", scaled)
print("centered:", centered)
print("sum:", values.sum())
print("transposed:\n", transposed)
```

## 3. When to use

* Když potřebujete rychlé numerické operace nad většími datovými poli.
* Když stavíte datový nebo ML workflow, kde jsou časté transformace dat.

## 4. Common mistakes

* ❌ Používání Python smyček nad `ndarray` místo vektorizace - výkon je pak výrazně horší.
* ❌ Nehlídané datové typy (`dtype`) - může dojít ke zbytečné spotřebě paměti nebo ztrátě přesnosti.

---

<details>
<summary>Optional: Deep dive</summary>

NumPy je velmi silný na CPU numeriku, ale není náhrada za databázi ani distribuovaný výpočet. U opravdu velkých datasetů je potřeba řešit chunking, out-of-core zpracování nebo specializované nástroje.

</details>

---

**Další krok:** [Pandas](Pandas.md)
