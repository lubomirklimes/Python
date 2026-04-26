# Kivy

Tato kapitola představuje Kivy, framework zaměřený na multiplatformní UI a dotykové ovládání.

## 1. Concept

Kivy je vhodné, když chcete jeden kód sdílet mezi desktopem a mobilem. Oproti Tkinteru nebo Qt má jiný styl UI a vlastní způsob definice rozhraní (Python + KV language). Silná stránka je multiplatformnost a práce s gesty.

Je dobré počítat s tím, že Kivy se vizuálně chová jinak než nativní widget toolkit dané platformy.

## 2. Example

```python
from kivy.app import App
from kivy.uix.boxlayout import BoxLayout
from kivy.uix.button import Button
from kivy.uix.label import Label


class Root(BoxLayout):
    def __init__(self, **kwargs):
        super().__init__(orientation="vertical", spacing=8, padding=12, **kwargs)
        self.label = Label(text="Ahoj z Kivy")
        button = Button(text="Změnit text")
        button.bind(on_press=self.on_press)
        self.add_widget(self.label)
        self.add_widget(button)

    def on_press(self, _instance) -> None:
        self.label.text = "Kliknuto"


class DemoApp(App):
    def build(self):
        return Root()


if __name__ == "__main__":
    DemoApp().run()
```

## 3. When to use

* Když cílíte na více platforem a chcete sdílet většinu UI logiky.
* Když potřebujete dotykové ovládání nebo kiosk-like aplikace.

## 4. Common mistakes

* ❌ Očekávání nativního vzhledu všech prvků bez úprav tématu.
* ❌ Nasazení bez testu na cílovém zařízení – chování na desktopu nemusí odpovídat mobilu.

---

<details>
<summary>Optional: Deep dive</summary>

Kivy má vlastní renderovací pipeline a event loop. U složitějších aplikací je dobré oddělit datovou vrstvu od UI a minimalizovat těžké výpočty v hlavním vlákně.

</details>

---

**Další krok:** [GUI architektura](GUI_architektura.md)
