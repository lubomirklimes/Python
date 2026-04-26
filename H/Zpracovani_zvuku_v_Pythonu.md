# Zpracování zvuku v Pythonu

Tato kapitola vysvětluje, jak se v Pythonu běžně pracuje se zvukem a kdy je to v praxi užitečné.

## 1. Concept

Ano, zpracování zvuku je v Pythonu běžné. Je standardní v oblastech jako:

- speech-to-text a voice assistants
- klasifikace zvuků (např. poruchy strojů)
- hudební analýza
- audio event detection

Typické knihovny:

- `librosa`: feature extraction (MFCC, mel-spektrogram)
- `soundfile` / `scipy`: čtení a zápis audia
- `torchaudio`: audio pipeline pro deep learning v PyTorch

Častý workflow:

- načtení wave signálu
- resampling a normalizace
- převod do spektrální reprezentace
- trénink/ inference modelu

## 2. Example

```python
from pathlib import Path
import librosa
import numpy as np
import soundfile as sf

AUDIO_PATH = Path("data/audio/sample.wav")
TARGET_SR = 16_000

signal, sr = sf.read(AUDIO_PATH, always_2d=False)
if signal.ndim > 1:
	signal = signal.mean(axis=1)  # stereo -> mono
signal = signal.astype(np.float32)

if sr != TARGET_SR:
	signal = librosa.resample(signal, orig_sr=sr, target_sr=TARGET_SR)
	sr = TARGET_SR

peak = float(np.max(np.abs(signal)))
if peak > 0:
	signal = signal / peak

# Jednoduché "feature" bez externích knihoven
mean_amp = float(np.mean(np.abs(signal)))
energy = float(np.mean(signal ** 2))
zero_crossings = int(np.sum(signal[:-1] * signal[1:] < 0))

mel = librosa.feature.melspectrogram(y=signal, sr=sr, n_fft=1024, hop_length=256, n_mels=64)
mfcc = librosa.feature.mfcc(y=signal, sr=sr, n_mfcc=20)

print("sample_rate:", sr)
print("mean_amp:", round(mean_amp, 4))
print("energy:", round(energy, 6))
print("zero_crossings:", zero_crossings)
print("mel shape:", mel.shape)
print("mfcc shape:", mfcc.shape)

# V praxi by následoval trénink nebo inference modelu.
```

## 3. When to use

* Když stavíte ASR, audio klasifikaci nebo detekci událostí v signálu.
* Když potřebujete analyzovat hlas, hudbu nebo technické zvuky ze zařízení.
* Když kombinujete multimodální data (obraz + zvuk + text).

## 4. Common mistakes

* ❌ Míchání různých sample rates bez resamplingu.
* ❌ Ignorování šumu a normalizace před extrakcí feature.
* ❌ Trénink modelu na jednom typu nahrávek a nasazení na výrazně jiném prostředí.

---

<details>
<summary>Optional: Deep dive</summary>

Audio pipeline má podobný princip jako CV: konzistentní preprocessing mezi tréninkem a inferencí. U audio projektů je navíc důležitá práce s okny, spektrogramy a robustnost na různé podmínky nahrávání.

</details>

---

**Další krok:** [Zvuková data a reprezentace signálu](Zvukova_data_a_reprezentace.md)
