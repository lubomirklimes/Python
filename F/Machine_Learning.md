# Machine Learning

Tato kapitola představuje základní workflow strojového učení v knihovně scikit-learn.

## 1. Concept

Scikit-learn je standardní knihovna pro klasické ML modely (regrese, klasifikace, clustering, preprocessing). Podporuje jasný pipeline přístup: příprava dat, rozdělení na trénink/test, trénink modelu, vyhodnocení metrik.

Důležité je měřit výkonnost na datech, která model neviděl při tréninku, jinak hrozí přeučení (overfitting).

## 2. Example

```python
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split
from sklearn.pipeline import make_pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import accuracy_score

X, y = load_iris(return_X_y=True)
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.25, random_state=42, stratify=y
)

model = make_pipeline(StandardScaler(), LogisticRegression(max_iter=500))
model.fit(X_train, y_train)

pred = model.predict(X_test)
print(f"Accuracy: {accuracy_score(y_test, pred):.3f}")
```

## 3. When to use

* Když řešíte predikce nad strukturovanými tabulkovými daty.
* Když potřebujete rychle postavit a porovnat několik klasických modelů.

## 4. Common mistakes

* ❌ Únik informací z test setu do tréninku (data leakage) - metriky pak vypadají lépe než realita.
* ❌ Hodnocení modelu jen jednou metrikou bez doménového kontextu.

---

<details>
<summary>Optional: Deep dive</summary>

U tabulkových dat bývají klasické modely (tree-based, linear) často dostatečné a levnější než deep learning. Začněte baseline modelem a teprve podle výsledků zvyšujte složitost.

</details>

---

**Další krok:** [Deep Learning](Deep_Learning.md)
