# scikit-learn

Tato kapitola ukazuje kompletní mini workflow ve scikit-learn: split, trénink, predikce, evaluace a pipeline.

## 1. Concept

scikit-learn je skvělá volba pro klasické ML nad tabulkovými daty. Poskytuje jednotné API pro modely i preprocessing. Nejčastěji řešíte:

- `train_test_split`
- `fit` / `predict`
- metriky (`accuracy`, `confusion_matrix`)
- pipeline (kombinace transformací a modelu)

Kdy použít scikit-learn místo TensorFlow: když řešíte strukturovaná data a nepotřebujete hluboké neuronové sítě.

## 2. Example

```python
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split
from sklearn.pipeline import make_pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import accuracy_score, confusion_matrix

X, y = load_iris(return_X_y=True)
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.25, random_state=42, stratify=y
)

pipeline = make_pipeline(StandardScaler(), LogisticRegression(max_iter=500))
pipeline.fit(X_train, y_train)

pred = pipeline.predict(X_test)
acc = accuracy_score(y_test, pred)
cm = confusion_matrix(y_test, pred)

print(f"accuracy={acc:.3f}")
print("confusion matrix:\n", cm)
```

## 3. When to use

* Když chcete rychle postavit baseline model nad tabulkovými daty.
* Když potřebujete čitelný a robustní pipeline pro preprocessing + model.
* Když chcete snadno porovnat více klasických algoritmů.

## 4. Common mistakes

* ❌ Aplikace scalingu mimo pipeline a nekonzistentně mezi train/test.
* ❌ Vyhodnocení modelu bez confusion matrix u klasifikace.
* ❌ Volba deep learningu bez důvodu, i když jednoduchý model stačí.

---

<details>
<summary>Optional: Deep dive</summary>

Pipeline koncept zabraňuje leakage a usnadňuje nasazení, protože transformace a model držíte jako jeden celek. To je často důležitější než honba za minimálním zlepšením metrik.

</details>

---

**Další krok:** [Trénink, validace a metriky modelu](Trenink_validace_a_metriky.md)
