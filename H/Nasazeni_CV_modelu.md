# Nasazení CV modelů

Tato kapitola pokrývá praktické nasazení computer vision modelu do API nebo streamovací služby.

## 1. Concept

Nasazení CV modelu znamená víc než jen uložit model na server. Musíte řešit:

- inference latenci (realtime vs batch)
- škálování služby
- stabilní preprocess pipeline
- monitoring kvality predikcí a driftu dat

Typické architektury:

- REST API pro obrázek na request
- dávkové zpracování fronty
- realtime stream z kamery

## 2. Example

```python
from fastapi import FastAPI, UploadFile, File, HTTPException
import cv2
import numpy as np
import tensorflow as tf

app = FastAPI()
MODEL_PATH = "models/cats_vs_dogs.keras"
IMAGE_SIZE = (128, 128)
LABELS = ["cat", "dog"]
MODEL_VERSION = "2026-04-cats-vs-dogs-v1"

model = tf.keras.models.load_model(MODEL_PATH)


def preprocess_image(image_bytes: bytes) -> np.ndarray:
    arr = np.frombuffer(image_bytes, dtype=np.uint8)
    image_bgr = cv2.imdecode(arr, cv2.IMREAD_COLOR)
    if image_bgr is None:
        raise HTTPException(status_code=400, detail="Soubor není validní obrázek")

    image_rgb = cv2.cvtColor(image_bgr, cv2.COLOR_BGR2RGB)
    image_resized = cv2.resize(image_rgb, IMAGE_SIZE, interpolation=cv2.INTER_AREA)
    image_norm = image_resized.astype(np.float32) / 255.0
    return np.expand_dims(image_norm, axis=0)


@app.post("/predict")
async def predict(file: UploadFile = File(...)) -> dict:
    if file.content_type not in {"image/jpeg", "image/png", "image/webp"}:
        raise HTTPException(status_code=415, detail="Podporovaný formát: JPEG, PNG, WEBP")

    content = await file.read()
    if not content:
        raise HTTPException(status_code=400, detail="Prázdný soubor")

    batch = preprocess_image(content)
    probs = model.predict(batch, verbose=0)[0]
    idx = int(np.argmax(probs))
    confidence = float(probs[idx])

    return {
        "label": LABELS[idx],
        "confidence": round(confidence, 3),
        "model_version": MODEL_VERSION,
    }
```

## 3. When to use

* Když převádíte experimentální notebook model do produkce.
* Když potřebujete obsloužit inferenci přes API pro více klientů.
* Když řešíte realtime zpracování obrazu v provozu.

## 4. Common mistakes

* ❌ Odlišný preprocess v produkci oproti tréninku.
* ❌ Chybějící limity velikosti vstupu a timeouty.
* ❌ Nasazení bez monitoringu latence a kvality predikcí.

---

<details>
<summary>Optional: Deep dive</summary>

U CV modelů je důležitá i hardwarová vrstva (CPU/GPU), batch size a optimalizace modelu (např. ONNX/TensorRT). Vhodná konfigurace může zásadně snížit latenci i náklady.

</details>

---

**Další krok:** [Zpracování zvuku v Pythonu](Zpracovani_zvuku_v_Pythonu.md)
