# Evaluace CV modelů v praxi

Tato kapitola ukazuje, jak hodnotit computer vision modely tak, aby metriky odpovídaly reálnému použití.

## 1. Concept

V CV nestačí jedna metrika. Podle typu úlohy používáte různé ukazatele:

- klasifikace: accuracy, precision, recall, F1, confusion matrix
- detekce: IoU, mAP
- segmentace: IoU/Dice score

Důležité je vyhodnocovat model na reprezentativních datech a dívat se i na konkrétní chybové případy. Čísla bez kontextu často zakryjí problémové scénáře.

## 2. Example

```python
import numpy as np
from sklearn.metrics import accuracy_score, precision_score, recall_score, f1_score, confusion_matrix

# y_prob může přijít přímo z model.predict(test_images)
y_true = np.array([1, 0, 1, 1, 0, 1, 0, 0], dtype=np.int64)
y_prob = np.array([0.91, 0.08, 0.84, 0.42, 0.27, 0.73, 0.62, 0.12], dtype=np.float32)

threshold = 0.5
y_pred = (y_prob >= threshold).astype(np.int64)

print("accuracy:", round(accuracy_score(y_true, y_pred), 3))
print("precision:", round(precision_score(y_true, y_pred), 3))
print("recall:", round(recall_score(y_true, y_pred), 3))
print("f1:", round(f1_score(y_true, y_pred), 3))
print("confusion matrix:\n", confusion_matrix(y_true, y_pred))


def iou(box_a: tuple[int, int, int, int], box_b: tuple[int, int, int, int]) -> float:
	ax1, ay1, ax2, ay2 = box_a
	bx1, by1, bx2, by2 = box_b

	inter_x1 = max(ax1, bx1)
	inter_y1 = max(ay1, by1)
	inter_x2 = min(ax2, bx2)
	inter_y2 = min(ay2, by2)

	inter_w = max(0, inter_x2 - inter_x1)
	inter_h = max(0, inter_y2 - inter_y1)
	inter_area = inter_w * inter_h

	area_a = (ax2 - ax1) * (ay2 - ay1)
	area_b = (bx2 - bx1) * (by2 - by1)
	union = area_a + area_b - inter_area
	return inter_area / union if union else 0.0


print("IoU example:", round(iou((20, 30, 120, 140), (40, 50, 150, 160)), 3))
```

## 3. When to use

* Když porovnáváte více verzí CV modelu před nasazením.
* Když řešíte, jestli je model dost spolehlivý pro konkrétní business scénář.
* Když nastavujete rozhodovací práh a potřebujete balanc mezi false positive a false negative.

## 4. Common mistakes

* ❌ Hodnocení jen přes accuracy bez analýzy precision/recall.
* ❌ Evaluace na nereprezentativním test setu.
* ❌ Ignorování edge cases (špatné světlo, rozmazání, jiná kamera).

---

<details>
<summary>Optional: Deep dive</summary>

U produkčních CV systémů se vyplatí rozdělit metriky podle segmentů (kamera, lokalita, čas, typ vstupu). Model může být silný celkově, ale slabý v kritické podmnožině.

</details>

---

**Další krok:** [Nasazení CV modelů](Nasazeni_CV_modelu.md)
