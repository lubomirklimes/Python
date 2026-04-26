# Realtime audio pipeline a nasazení

Tato kapitola uzavírá audio část: jak přejít od experimentu k provozní realtime pipeline.

## 1. Concept

Realtime audio systém zpracovává proud zvuku po krátkých framech a musí splnit latenci. Běžný tok:

- capture audio frame
- preprocess + feature extraction
- inference modelu
- decision/postprocessing

Klíčové provozní body:

- buffer a windowing strategie
- timeouty a backpressure
- monitoring latence a kvality
- drift dat v čase (jiné prostředí, mikrofony, šum)

## 2. Example

```python
import queue
import numpy as np
import librosa
import sounddevice as sd
import tensorflow as tf
import time

SAMPLE_RATE = 16_000
CHUNK_SECONDS = 1.0
CHUNK_SAMPLES = int(SAMPLE_RATE * CHUNK_SECONDS)
N_MELS = 64
MAX_FRAMES = 128
LABELS = ["background", "event"]

audio_queue: queue.Queue[np.ndarray] = queue.Queue(maxsize=32)
model = tf.keras.models.load_model("models/audio_event.keras")


def extract_features(samples: np.ndarray) -> np.ndarray:
    mel = librosa.feature.melspectrogram(
        y=samples,
        sr=SAMPLE_RATE,
        n_fft=1024,
        hop_length=256,
        n_mels=N_MELS,
    )
    mel_db = librosa.power_to_db(mel, ref=np.max)
    mel_db = (mel_db - mel_db.mean()) / (mel_db.std() + 1e-6)

    if mel_db.shape[1] < MAX_FRAMES:
        mel_db = np.pad(mel_db, ((0, 0), (0, MAX_FRAMES - mel_db.shape[1])), mode="constant")
    else:
        mel_db = mel_db[:, :MAX_FRAMES]

    return mel_db[..., np.newaxis].astype(np.float32)


def audio_callback(indata: np.ndarray, frames: int, time_info: dict, status: sd.CallbackFlags) -> None:
    if status:
        print("Audio callback warning:", status)
    mono = indata[:, 0].copy()
    try:
        audio_queue.put_nowait(mono)
    except queue.Full:
        pass

latencies = []

with sd.InputStream(
    samplerate=SAMPLE_RATE,
    channels=1,
    dtype="float32",
    blocksize=CHUNK_SAMPLES,
    callback=audio_callback,
):
    for _ in range(20):
        chunk = audio_queue.get(timeout=2.0)
        start = time.perf_counter()

        features = extract_features(chunk)
        batch = np.expand_dims(features, axis=0)
        probs = model.predict(batch, verbose=0)[0]
        pred_idx = int(np.argmax(probs))

        latency_ms = (time.perf_counter() - start) * 1000
        latencies.append(latency_ms)

        print("label:", LABELS[pred_idx], "confidence:", round(float(probs[pred_idx]), 3))

print("avg latency ms:", round(float(np.mean(latencies)), 2))
print("p95 latency ms:", round(float(np.percentile(latencies, 95)), 2))
```

## 3. When to use

* Když nasazujete audio model do streamovací nebo near-realtime služby.
* Když potřebujete stabilní latenci a robustnost na výpadky vstupu.
* Když chcete monitorovat kvalitu modelu i systémové metriky současně.

## 4. Common mistakes

* ❌ Návrh pipeline bez měření p95/p99 latence.
* ❌ Chybějící fallback při výpadku audio vstupu nebo modelu.
* ❌ Ignorování monitoringu kvality predikcí po nasazení.

---

<details>
<summary>Optional: Deep dive</summary>

Realtime audio je kompromis mezi kvalitou a latencí. Delší okna často zlepší stabilitu klasifikace, ale zhorší odezvu. Vhodný trade-off záleží na konkrétním use-casu.

</details>

---

**Další krok:** [Zpět na obsah](../README.md)
