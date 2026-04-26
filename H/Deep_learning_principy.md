# Deep learning - principy

Tato kapitola navazuje na klasický ML a vysvětluje, co je specifické pro deep learning v reálných projektech.

## 1. Concept

Deep learning je podmnožina machine learningu, kde model tvoří více vrstev neuronů. Díky tomu umí zachytit složité nelineární vztahy, zejména v obrazu, textu a zvuku.

Praktické stavební kameny:

- architektura sítě
- loss funkce a optimizer
- regularizace (dropout, weight decay)
- kontrola overfittingu (early stopping, validace)

DL má smysl, když máte dost dat a výpočetního výkonu. Jinak může být efektivnější jednodušší model.

## 2. Example

```python
from dataclasses import dataclass


@dataclass
class DlRunConfig:
    epochs: int
    batch_size: int
    learning_rate: float
    optimizer: str


def should_use_deep_learning(dataset_size: int, data_type: str) -> bool:
    return dataset_size > 10_000 and data_type in {"image", "text", "audio"}


cfg = DlRunConfig(epochs=30, batch_size=64, learning_rate=0.001, optimizer="adam")
print(cfg)
print("Použít DL:", should_use_deep_learning(50_000, "image"))
```

## 3. When to use

* Když řešíte úlohy nad obrazem, textem nebo sekvenčními daty.
* Když klasické modely nedokážou zachytit složitost problému.
* Když máte infrastrukturu pro trénink a monitoring modelu.

## 4. Common mistakes

* ❌ Deep learning bez baseline porovnání s jednoduššími modely.
* ❌ Nasazení bez monitoringu driftu dat a výkonu modelu.
* ❌ Trénink bez reprodukovatelného setupu (seed, verze závislostí, data snapshot).

---

<details>
<summary>Optional: Deep dive</summary>

V DL je důležitá i stabilita experimentů: verze dat, verze modelu a přesný popis hyperparametrů. Bez toho je těžké zopakovat úspěšný běh.

</details>

---

**Další krok:** [Typy neuronových sítí (MLP, CNN, RNN, Transformer)](Typy_neuronovych_siti.md)
