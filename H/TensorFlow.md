# TensorFlow / Keras

Tato kapitola ukazuje minimální binární klasifikaci v TensorFlow/Keras a vysvětluje trénink i predikci.

## 1. Concept

Keras (součást TensorFlow) umožňuje rychle skládat neuronové sítě pomocí `Sequential` modelu a vrstev `Dense`. U binární klasifikace je běžný výstupní neuron se sigmoid aktivací a loss `binary_crossentropy`.

Základní kroky:

- `compile`: zvolit optimizer, loss a metriky
- `fit`: trénink modelu na datech
- `predict`: predikce pravděpodobnosti třídy

Sledujte rozdíl train vs validation metrik. Pokud train stoupá, ale validation stagnuje nebo padá, typicky jde o overfitting.

## 2. Example

```python
import tensorflow as tf
from pathlib import Path

DATA_DIR = Path("data/cats_vs_dogs")
IMAGE_SIZE = (128, 128)
BATCH_SIZE = 32

train_ds = tf.keras.utils.image_dataset_from_directory(
    DATA_DIR,
    validation_split=0.2,
    subset="training",
    seed=42,
    image_size=IMAGE_SIZE,
    batch_size=BATCH_SIZE,
)
val_ds = tf.keras.utils.image_dataset_from_directory(
    DATA_DIR,
    validation_split=0.2,
    subset="validation",
    seed=42,
    image_size=IMAGE_SIZE,
    batch_size=BATCH_SIZE,
)

train_ds = train_ds.map(lambda x, y: (x / 255.0, y)).prefetch(tf.data.AUTOTUNE)
val_ds = val_ds.map(lambda x, y: (x / 255.0, y)).prefetch(tf.data.AUTOTUNE)
num_classes = len(train_ds.class_names)

model = tf.keras.Sequential(
    [
        tf.keras.layers.Input(shape=(*IMAGE_SIZE, 3)),
        tf.keras.layers.Conv2D(32, 3, activation="relu"),
        tf.keras.layers.MaxPooling2D(),
        tf.keras.layers.Conv2D(64, 3, activation="relu"),
        tf.keras.layers.MaxPooling2D(),
        tf.keras.layers.Conv2D(128, 3, activation="relu"),
        tf.keras.layers.Flatten(),
        tf.keras.layers.Dense(128, activation="relu"),
        tf.keras.layers.Dense(num_classes, activation="softmax"),
    ]
)

model.compile(optimizer="adam", loss="sparse_categorical_crossentropy", metrics=["accuracy"])
history = model.fit(train_ds, validation_data=val_ds, epochs=5)
loss, acc = model.evaluate(val_ds, verbose=0)

print("Poslední train accuracy:", round(float(history.history["accuracy"][-1]), 3))
print("Validation accuracy:", round(float(acc), 3))
print("Validation loss:", round(float(loss), 3))
```

## 3. When to use

* Když potřebujete flexibilní framework pro trénink neuronových sítí.
* Když řešíte klasifikaci/regresi, kde jednoduché modely nestačí.
* Když chcete přístup k bohatému ekosystému modelů a nástrojů.

## 4. Common mistakes

* ❌ Trénink bez validace a bez sledování overfittingu.
* ❌ Špatný loss pro daný typ úlohy (např. regresní loss pro klasifikaci).
* ❌ Interpretace výstupu sigmoid bez nastavení rozhodovacího prahu.

---

<details>
<summary>Optional: Deep dive</summary>

Keras je výborný pro rychlý prototyp, ale produkční model potřebuje navíc data versioning, model registry, monitoring driftu a reprodukovatelné tréninkové prostředí.

</details>

---

**Další krok:** [OpenCV pipeline a fáze zpracování obrazu](OpenCV_pipeline.md)
