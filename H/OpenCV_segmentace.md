# OpenCV: segmentace, hrany a kontury

Tato kapitola pokrývá další fázi obrazové pipeline: oddělení objektů od pozadí a extrakci tvarů.

## 1. Concept

Po základním preprocessingu často potřebujete získat strukturu obrazu:

- segmentace: oddělit relevantní oblasti
- hrany: zvýraznit přechody intenzity
- kontury: získat hranice objektů

Tyto metody se hodí samostatně i jako vstup pro další model.

## 2. Example

```python
import cv2
import numpy as np

img = np.zeros((200, 200), dtype=np.uint8)
cv2.rectangle(img, (50, 50), (150, 150), 255, -1)

# Hrany
edges = cv2.Canny(img, 50, 150)

# Thresholding a kontury
_, binary = cv2.threshold(img, 127, 255, cv2.THRESH_BINARY)
contours, _ = cv2.findContours(binary, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)

areas = [cv2.contourArea(c) for c in contours]
print("contours:", len(contours))
print("areas:", areas)
print("edge_pixels:", int((edges > 0).sum()))
```

## 3. When to use

* Když potřebujete najít objekty a jejich tvary bez složitého modelu.
* Když připravujete feature pro klasický CV nebo hybridní ML přístup.
* Když chcete předfiltrovat obraz před neuronovou sítí.

## 4. Common mistakes

* ❌ Nevhodné threshold parametry pro konkrétní typ obrazu.
* ❌ Ignorování šumu, který zhorší detekci hran a kontur.
* ❌ Převzetí univerzálních parametrů bez ladění na vlastních datech.

---

<details>
<summary>Optional: Deep dive</summary>

Kombinace Gaussian blur + adaptive threshold často zlepší robustnost segmentace. U průmyslových scénářů je klíčové měřit stabilitu na různém osvětlení.

</details>

---

**Další krok:** [OpenCV + neuronové sítě: klasifikace obrázků](OpenCV_a_neuronove_site.md)
