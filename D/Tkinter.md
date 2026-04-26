# Tkinter

Tato kapitola představuje základní GUI v Pythonu pomocí Tkinteru, který je součástí standardní instalace Pythonu.

## 1. Concept

Tkinter je nejjednodušší cesta, jak v Pythonu vytvořit desktopové okno s tlačítky, vstupními poli a textem. Je vhodný na interní nástroje, prototypy a menší aplikace, kde je důležitá rychlost vývoje a neřešíte výrazně moderní vzhled.

Základní princip je event-driven model: aplikace čeká na události (klik, stisk klávesy, změna hodnoty) a reaguje callback funkcemi.

## 2. Example

```python
import tkinter as tk
from tkinter import ttk


def on_click() -> None:
    name = name_var.get().strip() or "světe"
    output_var.set(f"Ahoj, {name}!")


root = tk.Tk()
root.title("Tkinter demo")
root.geometry("320x160")

name_var = tk.StringVar()
output_var = tk.StringVar(value="Zadej jméno a klikni.")

frame = ttk.Frame(root, padding=12)
frame.pack(fill="both", expand=True)

entry = ttk.Entry(frame, textvariable=name_var)
entry.pack(fill="x", pady=4)

button = ttk.Button(frame, text="Pozdrav", command=on_click)
button.pack(pady=4)

label = ttk.Label(frame, textvariable=output_var)
label.pack(pady=4)

root.mainloop()
```

## 3. When to use

* Když potřebujete rychle vytvořit jednoduché desktopové rozhraní bez externích závislostí.
* Když děláte interní utility pro tým (konfigurátory, import/export nástroje).

## 4. Common mistakes

* ❌ Těžká logika přímo v callbacku tlačítka – GUI pak zamrzá a kód se špatně udržuje.
* ❌ Ignorování layoutu a škálování – okno je nepoužitelné na jiných rozlišeních.

---

<details>
<summary>Optional: Deep dive</summary>

Tkinter je synchronní a běží v hlavním vlákně. Pokud potřebujete dělat delší operace (síť, čtení velkých souborů), spouštějte je mimo hlavní smyčku a výsledek vracejte bezpečně zpět do UI.

</details>

---

**Další krok:** [PyQt / PySide](PyQt_PySide.md)
