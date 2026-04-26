# Anotace dat a tvorba datasetu pro CV

Tato kapitola vysvětluje, jak správně připravit anotovaný dataset pro computer vision modely.

## 1. Concept

Kvalita anotací často rozhoduje víc než volba architektury modelu. U CV typicky řešíte:

- klasifikaci (label na celý obrázek)
- detekci objektů (bounding box)
- segmentaci (pixelová maska)

Praktický postup:

- definovat jasné třídy a pravidla anotace
- oddělit train/validation/test už na začátku
- hlídat distribuci tříd a kvalitu anotací
- držet konzistentní formát (např. COCO, YOLO)

Bez kvalitního datasetu vzniká šum, který model přejímá a následně selhává v produkci.

## 2. Example

```python
from dataclasses import dataclass
from collections import Counter


@dataclass
class Annotation:
    image_id: str
    label: str


annotations = [
    Annotation("img_001", "cat"),
    Annotation("img_002", "dog"),
    Annotation("img_003", "cat"),
    Annotation("img_004", "dog"),
    Annotation("img_005", "dog"),
]

label_distribution = Counter(a.label for a in annotations)
print("Distribuce tříd:", dict(label_distribution))

# Jednoduchá kontrola nevyváženosti
max_count = max(label_distribution.values())
min_count = min(label_distribution.values())
print("Imbalance ratio:", round(max_count / min_count, 2))
```

## 3. When to use

* Když stavíte image klasifikaci, detekci objektů nebo segmentaci.
* Když migrace modelu na produkční data dává slabé výsledky a je potřeba audit datasetu.
* Když připravujete dlouhodobě udržovatelný datový proces pro retrénink.

## 4. Common mistakes

* ❌ Nejasná pravidla anotace mezi anotátory - model se učí nekonzistentní labely.
* ❌ Náhodný split dat bez hlídání úniku podobných obrázků mezi train a test.
* ❌ Ignorování nevyváženosti tříd a kvality validace.

---

<details>
<summary>Optional: Deep dive</summary>

U složitějších projektů je vhodné měřit i inter-annotator agreement a zavést kontrolní audit vzorku anotací. Tím odhalíte systematické chyby dřív, než investujete čas do tréninku.

</details>

---

**Další krok:** [Evaluace CV modelů v praxi](Evaluace_CV_modelu.md)
