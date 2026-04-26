# Základy machine learningu v praxi

Tato kapitola vysvětluje základní pojmy machine learningu a jak přemýšlet o kvalitě modelu.

## 1. Concept

Machine learning je přístup, kdy model hledá vzory v datech místo ručně napsaných pravidel. Model nevrací absolutní pravdu, ale statistický odhad založený na tom, co se naučil z tréninkových dat.

Základní dělení:

- supervised learning: máme features + správné odpovědi (labels)
- unsupervised learning: máme data bez labelů, hledáme strukturu

Typy úloh:

- klasifikace: výstup je třída (např. spam/ham)
- regrese: výstup je číslo (např. odhad ceny)

Workflow:

- train: model se učí
- validation: ladění hyperparametrů
- test: finální nezávislé vyhodnocení

Overfitting = model se naučí trénink „nazpaměť“. Underfitting = model je příliš jednoduchý a neumí zachytit signál.

## 2. Example

```python
from dataclasses import dataclass


@dataclass
class DatasetSplit:
    train_ratio: float
    val_ratio: float
    test_ratio: float


def describe_ml_task(task: str, supervised: bool) -> str:
    mode = "supervised" if supervised else "unsupervised"
    return f"Úloha: {task}, režim: {mode}"


split = DatasetSplit(train_ratio=0.7, val_ratio=0.15, test_ratio=0.15)
print(describe_ml_task("klasifikace", supervised=True))
print(split)
print("Pozn.: model se učí vzory z dat, ne jistotu pravdy.")
```

## 3. When to use

* Když máte dost historických dat a chcete predikovat nebo klasifikovat nové případy.
* Když ručně psaná pravidla nestačí na variabilitu problému.
* Když dokážete definovat metriky, podle kterých poznáte úspěch modelu.

## 4. Common mistakes

* ❌ Spuštění modelu bez jasně definované metriky úspěchu.
* ❌ Vyhodnocení jen na train datech bez validation/test splitu.
* ❌ Přesvědčení, že vyšší accuracy automaticky znamená business hodnotu.

---

<details>
<summary>Optional: Deep dive</summary>

Metrika musí odpovídat cíli. U nevyvážených tříd může být accuracy zavádějící; často jsou lepší precision/recall/F1 nebo ROC-AUC. Model je nástroj rozhodování, ne náhrada doménového kontextu.

</details>

---

**Další krok:** [scikit-learn v praxi](Scikit_learn.md)
