# OpenCV pipeline a fáze zpracování obrazu

Tato kapitola ukazuje základy práce s obrázky a videem v OpenCV a vazbu na NumPy.

## 1. Concept

OpenCV je knihovna pro zpracování obrazu a videa v reálném čase. Obrázek je v OpenCV reprezentovaný jako NumPy pole. To znamená, že můžete kombinovat OpenCV operace s NumPy transformacemi.

Typická CV pipeline má několik fází:

- načtení a validace vstupu
- předzpracování (resize, normalizace, denoise)
- segmentace nebo detekce hran/regionů
- extrakce feature nebo předání do neuronové sítě
- inference a postprocessing výsledku

Důležité body:

- OpenCV načítá barevné obrázky jako BGR, ne RGB
- běžné operace: grayscale, resize, crop, edge detection, thresholding
- video/kamera se zpracovává frame po frame

## 2. Example

```python
import cv2
import numpy as np

# Vytvoření umělého obrázku (NumPy array)
img = np.zeros((200, 300, 3), dtype=np.uint8)
img[:, :] = (20, 120, 220)  # BGR barva

# BGR -> RGB (např. pro matplotlib)
rgb = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)

gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
resized = cv2.resize(gray, (150, 100))
crop = img[50:150, 80:220]

edges = cv2.Canny(gray, threshold1=50, threshold2=150)
_, binary = cv2.threshold(gray, 100, 255, cv2.THRESH_BINARY)

print("img shape:", img.shape)
print("rgb shape:", rgb.shape)
print("resized shape:", resized.shape)
print("crop shape:", crop.shape)
print("edges nonzero:", int((edges > 0).sum()))
print("binary unique:", np.unique(binary))

# Video/kamera koncept:
# cap = cv2.VideoCapture(0)
# ret, frame = cap.read()
# cap.release()
```

## 3. When to use

* Když potřebujete preprocess obrázky před ML modelem.
* Když děláte detekci hran, segmentaci nebo jednoduché CV operace.
* Když zpracováváte stream z kamery nebo videa.

## 4. Common mistakes

* ❌ Zaměnění BGR a RGB při vizualizaci nebo předávání do modelu.
* ❌ Trénink modelu na jiném preprocessingu než při inference.
* ❌ Ignorování výkonu při zpracování videa frame-by-frame.

---

<details>
<summary>Optional: Deep dive</summary>

OpenCV umí jak klasické heuristické metody, tak přípravu dat pro deep learning modely. V praxi se často kombinuje rychlá klasická filtrace (crop, threshold) s následnou neuronovou klasifikací.

</details>

---

**Další krok:** [OpenCV: načtení, předzpracování a augmentace](OpenCV_predzpracovani.md)
