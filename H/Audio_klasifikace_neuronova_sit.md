# Audio klasifikace s neuronovou sítí

Tato kapitola ukazuje základní koncept klasifikace zvuku pomocí neuronové sítě.

## 1. Concept

U audio klasifikace model predikuje třídu zvukové ukázky (např. "siréna", "řeč", "hudba"). V praxi model často dostává jako vstup mel-spektrogram nebo MFCC tensor.

Pipeline:

- načíst audio
- vytvořit feature reprezentaci
- natrénovat model (CNN/MLP)
- vyhodnotit přes precision/recall/F1

Pro malé datasety je důležitá augmentace (noise, pitch shift, time stretch), aby model generalizoval.

## 2. Example

```python
from pathlib import Path
import librosa
import numpy as np
import tensorflow as tf

SAMPLE_RATE = 16_000
N_MELS = 64
MAX_FRAMES = 128
LABEL_TO_ID = {"background": 0, "siren": 1, "speech": 2}
ID_TO_LABEL = {v: k for k, v in LABEL_TO_ID.items()}


def extract_mel(path: Path) -> np.ndarray:
	y, sr = librosa.load(path, sr=SAMPLE_RATE, mono=True)
	mel = librosa.feature.melspectrogram(y=y, sr=sr, n_fft=1024, hop_length=256, n_mels=N_MELS)
	mel_db = librosa.power_to_db(mel, ref=np.max)
	mel_db = (mel_db - mel_db.mean()) / (mel_db.std() + 1e-6)

	if mel_db.shape[1] < MAX_FRAMES:
		pad = MAX_FRAMES - mel_db.shape[1]
		mel_db = np.pad(mel_db, ((0, 0), (0, pad)), mode="constant")
	else:
		mel_db = mel_db[:, :MAX_FRAMES]

	return mel_db[..., np.newaxis].astype(np.float32)


files = [
	Path("data/audio/train/background_001.wav"),
	Path("data/audio/train/siren_001.wav"),
	Path("data/audio/train/speech_001.wav"),
]
labels = ["background", "siren", "speech"]

X = np.stack([extract_mel(path) for path in files], axis=0)
y = np.array([LABEL_TO_ID[label] for label in labels], dtype=np.int64)

model = tf.keras.Sequential(
	[
		tf.keras.layers.Input(shape=(N_MELS, MAX_FRAMES, 1)),
		tf.keras.layers.Conv2D(32, 3, activation="relu"),
		tf.keras.layers.MaxPooling2D(),
		tf.keras.layers.Conv2D(64, 3, activation="relu"),
		tf.keras.layers.MaxPooling2D(),
		tf.keras.layers.Flatten(),
		tf.keras.layers.Dense(64, activation="relu"),
		tf.keras.layers.Dense(len(LABEL_TO_ID), activation="softmax"),
	]
)
model.compile(optimizer="adam", loss="sparse_categorical_crossentropy", metrics=["accuracy"])
model.fit(X, y, epochs=10, batch_size=2, verbose=0)

probs = model.predict(X[:1], verbose=0)[0]
pred_idx = int(np.argmax(probs))
confidence = float(probs[pred_idx])

print("samples:", X.shape[0])
print("input shape:", X.shape[1:])
print("prediction:", ID_TO_LABEL[pred_idx])
print("confidence:", round(confidence, 3))
```

## 3. When to use

* Když potřebujete rozlišovat typy zvukových událostí.
* Když stavíte audio monitoring (průmysl, bezpečnost, asistivní aplikace).
* Když chcete použít stejné principy jako v CV, ale nad audio reprezentací.

## 4. Common mistakes

* ❌ Nerovnováha tříd bez vyvážení datasetu nebo metrik.
* ❌ Trénink na čistých datech a nasazení v hlučném prostředí bez augmentace.
* ❌ Hodnocení jen accuracy bez confusion matrix a per-class metrik.

---

<details>
<summary>Optional: Deep dive</summary>

Mnoho audio modelů dnes využívá transfer learning z velkých předtrénovaných embedding modelů. To může výrazně pomoci tam, kde máte málo vlastních anotovaných dat.

</details>

---

**Další krok:** [Realtime audio pipeline a nasazení](Realtime_audio_pipeline.md)
