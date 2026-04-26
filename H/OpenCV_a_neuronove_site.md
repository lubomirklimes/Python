# AI + obraz - kombinace OpenCV a neuronových sítí

Tato kapitola spojuje zpracování obrazu a neuronové modely do jedné praktické pipeline.

## 1. Concept

V image AI workflow nejdřív připravíte obraz (preprocessing), pak ho převedete do numerické reprezentace (NumPy array) a až potom pošlete do modelu.

Typická pipeline:

image -> preprocessing -> model -> prediction

Preprocessing obvykle zahrnuje:

- načtení obrázku
- resize na vstupní velikost modelu
- normalizaci pixelů (např. 0-255 -> 0.0-1.0)
- úpravu tvaru tensoru (batch dimension)

Rozdíl mezi ručním zpracováním obrazu a neuronovou sítí:

- ruční metody (OpenCV) používají explicitní pravidla
- neuronová síť se učí reprezentace z dat

## 2. Example

```python
import cv2
import numpy as np
import tensorflow as tf
from pathlib import Path

MODEL_PATH = Path("models/cats_vs_dogs.keras")
IMAGE_PATH = Path("data/inference/sample.jpg")
LABELS = ["cat", "dog"]


def preprocess_image(path: Path, size: tuple[int, int] = (128, 128)) -> np.ndarray:
	image_bgr = cv2.imread(str(path))
	if image_bgr is None:
		raise FileNotFoundError(f"Obrázek nebyl nalezen: {path}")

	image_rgb = cv2.cvtColor(image_bgr, cv2.COLOR_BGR2RGB)
	image_resized = cv2.resize(image_rgb, size, interpolation=cv2.INTER_AREA)
	image_norm = image_resized.astype(np.float32) / 255.0
	return np.expand_dims(image_norm, axis=0)


model = tf.keras.models.load_model(MODEL_PATH)
batch = preprocess_image(IMAGE_PATH)
probs = model.predict(batch, verbose=0)[0]

predicted_class = int(np.argmax(probs))
confidence = float(probs[predicted_class])

print("Input batch shape:", batch.shape)
print("Predicted class:", LABELS[predicted_class])
print("Confidence:", round(confidence, 3))

if confidence < 0.6:
	print("Pozor: nízká jistota, výsledek pošlete na manuální kontrolu.")
```

## 3. When to use

* Když potřebujete klasifikovat obrázky v produkční pipeline.
* Když kombinujete OpenCV preprocessing s TensorFlow/PyTorch modelem.
* Když stavíte CV aplikace jako kontrola kvality, OCR pre-step nebo detekce objektů.

## 4. Common mistakes

* ❌ Inference používá jiný preprocessing než trénink dat.
* ❌ Vynechání normalizace nebo chybné pořadí kanálů (BGR/RGB).
* ❌ Přímá interpretace výstupu modelu bez prahu a validace na reálných datech.

---

<details>
<summary>Optional: Deep dive</summary>

V produkci je důležité monitorovat distribuci vstupních obrázků a kvalitu predikcí. Model může degradovat při změně světelných podmínek, kamery nebo doménového posunu dat.

</details>

---

**Další krok:** [End-to-end mini projekt: OpenCV + CNN](OpenCV_CNN_mini_projekt.md)
