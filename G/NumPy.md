# NumPy - práce s poli

Tato kapitola vysvětluje, jak NumPy pracuje s poli, proč je rychlý a jak zapadá do ekosystému datové vědy.

## 1. Concept

NumPy je základní stavební kámen většiny datových a AI knihoven v Pythonu. Hlavní objekt `ndarray` ukládá hodnoty kompaktně v paměti a umožňuje rychlé vektorové operace nad celým polem.

Důležité pojmy:

- `ndarray`: n-rozměrné pole stejného datového typu
- broadcasting: pravidla, která dovolí operace mezi poli různého tvaru
- vektorové operace: výpočty nad celou strukturou bez explicitní Python smyčky

Proč je NumPy rychlejší než běžné listy:

- homogenní data -> menší režie objektů
- interní implementace v C
- méně Python-level iterace

NumPy je základ pro Pandas tabulky, mnoho ML modelů i OpenCV obrazová data (obrázek je v praxi často NumPy pole).

## 2. Example

```python
import numpy as np

# ndarray
x = np.array([1.0, 2.0, 3.0, 4.0])
y = np.array([10.0, 20.0, 30.0, 40.0])

# vektorové operace
z = x * 2 + y

# broadcasting: (2, 3) + (3,)
matrix = np.array([[1, 2, 3], [4, 5, 6]], dtype=np.float64)
bias = np.array([0.1, 0.2, 0.3])
shifted = matrix + bias

print("z:", z)
print("shifted:\n", shifted)
print("mean:", shifted.mean())
```

## 3. When to use

* Když potřebujete dělat rychlé numerické transformace nad větším množstvím dat.
* Když připravujete data pro machine learning nebo počítačové vidění.
* Když chcete nahradit pomalé Python smyčky vektorovými operacemi.

## 4. Common mistakes

* ❌ Iterace přes každý prvek v Python smyčce místo vektorizace.
* ❌ Nehlídaný `dtype`, který zvyšuje paměť nebo způsobí nečekané přetypování.
* ❌ Ignorování shape polí - operace selže nebo vrátí nežádoucí výsledek.

---

<details>
<summary>Optional: Deep dive</summary>

Broadcasting je velmi silný, ale je nutné rozumět rozměrům. Obecně se porovnávají dimenze zprava doleva a velikost musí být stejná, nebo jedna z nich 1. Tento princip je klíčový i v neuronových sítích.

</details>

---

**Další krok:** [Pandas - práce s daty](Pandas.md)
