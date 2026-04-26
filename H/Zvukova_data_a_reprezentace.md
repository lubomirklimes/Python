# Zvuková data a reprezentace signálu

Tato kapitola vysvětluje, jak je zvuk reprezentovaný digitálně a proč jsou parametry signálu zásadní pro další zpracování.

## 1. Concept

Digitální audio je časová řada amplitud vzorkovaných v čase. Pro správné zpracování potřebujete rozumět těmto pojmům:

- sample rate (Hz): kolik vzorků za sekundu
- bit depth: přesnost amplitudy
- mono/stereo: počet kanálů
- duration: délka nahrávky

Pro ML i DSP je důležité mít konzistentní sample rate a kanálový formát. Různé zdroje audia (mikrofony, telefony, web) často přichází v různých parametrech.

## 2. Example

```python
import numpy as np

sr = 16_000  # sample rate
seconds = 2
t = np.linspace(0, seconds, sr * seconds, endpoint=False)

# testovací signál: 440 Hz + slabší 880 Hz
signal = 0.7 * np.sin(2 * np.pi * 440 * t) + 0.2 * np.sin(2 * np.pi * 880 * t)

print("samples:", signal.shape[0])
print("duration_s:", signal.shape[0] / sr)
print("min/max:", float(signal.min()), float(signal.max()))
print("dtype:", signal.dtype)
```

## 3. When to use

* Když připravujete audio data před feature extraction.
* Když ladíte problémy s nekonzistentním vstupním formátem.
* Když stavíte pipeline pro speech nebo audio klasifikaci.

## 4. Common mistakes

* ❌ Míchání více sample rates bez explicitního resamplingu.
* ❌ Ignorování více kanálů (stereo) při očekávání mono signálu.
* ❌ Práce s clipping signálem bez normalizace.

---

<details>
<summary>Optional: Deep dive</summary>

V audio světě je běžná kompromisní volba 16 kHz pro speech use-cases, zatímco hudba často vyžaduje vyšší sample rate. Rozhodnutí ovlivní kvalitu i výpočetní nároky.

</details>

---

**Další krok:** [Audio feature engineering (MFCC, mel-spektrogram)](Audio_feature_engineering.md)
