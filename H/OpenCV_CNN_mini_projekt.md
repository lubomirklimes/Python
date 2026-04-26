# End-to-end mini projekt: OpenCV + CNN

Tato kapitola propojuje předchozí kroky do jednoho praktického mini projektu pro třídění obrázků.

## 1. Concept

Cíl mini projektu je ukázat kompletní tok:

- načíst obrázek
- preprocessing přes OpenCV
- převod na tensor pro model
- predikce CNN
- interpretace výstupu

Důležité je, že stejný preprocess musí platit pro trénink i inferenci.

## 2. Example

```python
import cv2
import numpy as np
import tensorflow as tf
from pathlib import Path

DATA_DIR = Path("data/cats_vs_dogs")
IMAGE_SIZE = (128, 128)
BATCH_SIZE = 32
MODEL_PATH = Path("models/cats_vs_dogs.keras")
INFERENCE_IMAGE = Path("data/inference/sample.jpg")


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
class_names = train_ds.class_names

train_ds = train_ds.map(lambda x, y: (x / 255.0, y)).prefetch(tf.data.AUTOTUNE)
val_ds = val_ds.map(lambda x, y: (x / 255.0, y)).prefetch(tf.data.AUTOTUNE)

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
		tf.keras.layers.Dense(len(class_names), activation="softmax"),
	]
)
model.compile(optimizer="adam", loss="sparse_categorical_crossentropy", metrics=["accuracy"])
model.fit(train_ds, validation_data=val_ds, epochs=5)
model.save(MODEL_PATH)


image_bgr = cv2.imread(str(INFERENCE_IMAGE))
if image_bgr is None:
	raise FileNotFoundError(f"Obrázek nebyl nalezen: {INFERENCE_IMAGE}")

image_rgb = cv2.cvtColor(image_bgr, cv2.COLOR_BGR2RGB)
image_resized = cv2.resize(image_rgb, IMAGE_SIZE, interpolation=cv2.INTER_AREA)
batch = np.expand_dims(image_resized.astype(np.float32) / 255.0, axis=0)
probs = model.predict(batch, verbose=0)[0]
idx = int(np.argmax(probs))

print("Predikce:", class_names[idx])
print("Confidence:", round(float(probs[idx]), 3))
```

## 3. When to use

* Když chcete ověřit celý CV + DL workflow na malém prototypu.
* Když připravujete podklad pro produkční image klasifikaci.
* Když učíte tým principy od preprocessingu po predikci.

## 4. Common mistakes

* ❌ Přeskok validace datové pipeline mezi tréninkem a inferencí.
* ❌ Chybějící monitoring confidence a chybových případů.
* ❌ Nasazení bez testu na reálných produkčních datech.

---

<details>
<summary>Optional: Deep dive</summary>

Po mini projektu následuje typicky MLOps vrstva: verzování modelu, reproducibilní trénink, monitoring driftu a pravidelný retraining. Bez toho kvalita modelu v čase klesá.

</details>

---

**Další krok:** [Anotace dat a tvorba datasetu pro CV](CV_anotace_a_datasety.md)
