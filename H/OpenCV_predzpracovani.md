# OpenCV: načtení, předzpracování a augmentace

Tato kapitola rozděluje první fáze práce s obrazem: načtení, normalizaci, resize a jednoduché augmentace.

## 1. Concept

Předzpracování obrazu je kritické pro kvalitu modelu. Cílem je dát modelu konzistentní vstup:

- jednotná velikost obrázků
- správný barevný prostor
- normalizovaný rozsah pixelů
- případná augmentace pro robustnost modelu

Augmentace (flip, rotate, brightness) pomáhá modelu generalizovat, ale nesmí porušit význam dat.

## 2. Example

```python
import cv2
import numpy as np
from pathlib import Path

IMAGE_PATH = Path("data/raw/produkt_001.jpg")

img_bgr = cv2.imread(str(IMAGE_PATH))
if img_bgr is None:
	raise FileNotFoundError(f"Obrázek nebyl nalezen: {IMAGE_PATH}")

resized = cv2.resize(img_bgr, (128, 128), interpolation=cv2.INTER_AREA)
rgb = cv2.cvtColor(resized, cv2.COLOR_BGR2RGB)
normalized = rgb.astype(np.float32) / 255.0

# Jednoduché augmentace
flipped = cv2.flip(rgb, 1)  # horizontální flip
bright = cv2.convertScaleAbs(rgb, alpha=1.1, beta=15)  # zvýšení jasu

print("normalized min/max:", float(normalized.min()), float(normalized.max()))
print("flipped shape:", flipped.shape)
print("bright shape:", bright.shape)
```

## 3. When to use

* Když připravujete obrázky pro trénink CNN modelu.
* Když chcete sjednotit vstupní data z různých zdrojů.
* Když potřebujete zvýšit robustnost modelu proti variacím obrazu.

## 4. Common mistakes

* ❌ Jiný preprocessing pro trénink a inference.
* ❌ Přehnaná augmentace, která mění význam třídy.
* ❌ Zapomenutí na BGR vs RGB konverzi.

---

<details>
<summary>Optional: Deep dive</summary>

V produkci se vyplatí mít preprocess pipeline jako explicitní modul s testy. Tím snížíte riziko, že se preprocess nechtěně změní při refaktoringu.

</details>

---

**Další krok:** [OpenCV: segmentace, hrany a kontury](OpenCV_segmentace.md)
