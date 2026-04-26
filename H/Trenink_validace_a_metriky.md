# Trénink, validace a metriky modelu

Tato kapitola ukazuje, jak správně vyhodnocovat model a proč nestačí dívat se jen na jednu metriku.

## 1. Concept

Trénink modelu není jen o `fit()`. Musíte vědět, jak se model chová na validačních a testovacích datech. Typický proces:

- train set: model se učí parametry
- validation set: ladění hyperparametrů
- test set: finální nezávislé ověření

Metriky volte podle úlohy:

- klasifikace: accuracy, precision, recall, F1, confusion matrix
- regrese: MAE, RMSE, R2

## 2. Example

```python
from sklearn.datasets import load_breast_cancer
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import accuracy_score, precision_score, recall_score, confusion_matrix

X, y = load_breast_cancer(return_X_y=True)
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42, stratify=y
)

model = LogisticRegression(max_iter=2000)
model.fit(X_train, y_train)
pred = model.predict(X_test)

print("accuracy:", round(accuracy_score(y_test, pred), 3))
print("precision:", round(precision_score(y_test, pred), 3))
print("recall:", round(recall_score(y_test, pred), 3))
print("confusion matrix:\n", confusion_matrix(y_test, pred))
```

## 3. When to use

* Když ladíte model a potřebujete objektivně porovnat varianty.
* Když rozhodujete o nasazení modelu do produkce.
* Když potřebujete vysvětlit trade-off mezi false positive a false negative.

## 4. Common mistakes

* ❌ Hodnocení modelu jen na train datech.
* ❌ Použití pouze accuracy u nevyvážených tříd.
* ❌ Míchání validačního a testovacího setu při ladění.

---

<details>
<summary>Optional: Deep dive</summary>

U produkčních modelů je důležitá i kalibrace pravděpodobností a výběr rozhodovacího prahu. Ten by měl odpovídat nákladům chyb v reálném procesu.

</details>

---

**Další krok:** [Deep learning - principy](Deep_learning_principy.md)
