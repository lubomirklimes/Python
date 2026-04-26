# PyQt / PySide

Tato kapitola ukazuje modernější desktop GUI přístup přes Qt framework dostupný v Pythonu jako PyQt nebo PySide.

## 1. Concept

PyQt/PySide staví nad Qt, které nabízí bohaté widgety, kvalitní layout systém, témata a pokročilé prvky (tabulky, grafika, model/view architektura). Oproti Tkinteru je vhodnější pro větší aplikace s náročnějším UI.

PyQt a PySide jsou si API velmi podobné. Rozdíl bývá hlavně v licenci a distribuci. Pro týmový projekt je dobré vybrat jednu variantu a držet se jí konzistentně.

## 2. Example

```python
import sys
from PySide6.QtWidgets import QApplication, QWidget, QVBoxLayout, QLabel, QLineEdit, QPushButton


class MainWindow(QWidget):
    def __init__(self) -> None:
        super().__init__()
        self.setWindowTitle("PySide demo")

        self.input = QLineEdit()
        self.output = QLabel("Zadej jméno.")
        button = QPushButton("Pozdrav")
        button.clicked.connect(self.say_hello)

        layout = QVBoxLayout()
        layout.addWidget(self.input)
        layout.addWidget(button)
        layout.addWidget(self.output)
        self.setLayout(layout)

    def say_hello(self) -> None:
        name = self.input.text().strip() or "světe"
        self.output.setText(f"Ahoj, {name}!")


app = QApplication(sys.argv)
window = MainWindow()
window.show()
app.exec()
```

## 3. When to use

* Když stavíte desktop aplikaci, která má delší životnost a více obrazovek.
* Když potřebujete profesionálnější UI prvky, tabulky nebo pokročilé formuláře.

## 4. Common mistakes

* ❌ Smíchání business logiky a UI třídy bez vrstev – aplikace je těžko testovatelná.
* ❌ Podcenění licenčních podmínek při výběru mezi PyQt a PySide.

---

<details>
<summary>Optional: Deep dive</summary>

Qt dobře podporuje signal/slot architekturu a model-view přístup. U větších aplikací se osvědčuje oddělení doménové logiky do služeb a UI držet co nejtenčí.

</details>

---

**Další krok:** [Kivy](Kivy.md)
