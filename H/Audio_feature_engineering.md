# Audio feature engineering (MFCC, mel-spektrogram)

Tato kapitola ukazuje, jak převést surový zvuk do reprezentace vhodné pro modely.

## 1. Concept

Surový waveform je pro mnoho modelů méně praktický než spektrální reprezentace. Nejčastější features:

- mel-spektrogram: energie frekvencí v čase na mel škále
- MFCC: kompaktní koeficienty odvozené ze spektra
- zero-crossing rate, energy, spectral centroid

Typický tok je: waveform -> framing/windowing -> spektrum -> feature tensor.

## 2. Example

```python
from pathlib import Path
import librosa
import numpy as np

AUDIO_PATH = Path("data/audio/sample.wav")
TARGET_SR = 16_000

y, sr = librosa.load(AUDIO_PATH, sr=TARGET_SR, mono=True)

mel = librosa.feature.melspectrogram(y=y, sr=sr, n_fft=1024, hop_length=256, n_mels=64)
mel_db = librosa.power_to_db(mel, ref=np.max)

mfcc = librosa.feature.mfcc(y=y, sr=sr, n_mfcc=20, n_fft=1024, hop_length=256)
mfcc_delta = librosa.feature.delta(mfcc)
mfcc_delta2 = librosa.feature.delta(mfcc, order=2)

# Sloučení MFCC + delta + delta-delta do jednoho feature tensoru
features = np.concatenate([mfcc, mfcc_delta, mfcc_delta2], axis=0).T.astype(np.float32)

print("sample_rate:", sr)
print("signal seconds:", round(len(y) / sr, 2))
print("mel_db shape:", mel_db.shape)          # (n_mels, time_steps)
print("mfcc stack shape:", features.shape)    # (time_steps, 60)
```

## 3. When to use

* Když připravujete vstupy pro klasifikaci zvuku nebo speech modely.
* Když chcete snížit dimenzi a zvýšit robustnost vstupu.
* Když potřebujete porovnávat různé feature sety pro stejný problém.

## 4. Common mistakes

* ❌ Nekonzistentní parametry framingu mezi train a inference.
* ❌ Použití feature bez normalizace napříč datasetem.
* ❌ Přehnané množství feature bez validace přínosu.

---

<details>
<summary>Optional: Deep dive</summary>

MFCC jsou tradičně silné pro speech scénáře, ale u některých problémů dnes lépe fungují learnované reprezentace z raw nebo mel-spektrogramů přes CNN/Transformer architektury.

</details>

---

**Další krok:** [Audio klasifikace s neuronovou sítí](Audio_klasifikace_neuronova_sit.md)
