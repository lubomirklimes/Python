# GUI architektura

Tato kapitola shrnuje event-driven model a doporučenou architekturu GUI aplikací.

## 1. Concept

GUI aplikace jsou řízené událostmi. Uživatel kliká, píše nebo mění hodnoty a framework volá vaše callbacky. Proto je důležité oddělit:

- prezentační vrstvu (okna, widgety, formuláře)
- aplikační logiku (validace, workflow)
- datovou vrstvu (soubor, DB, API)

Toto rozdělení zlepší testovatelnost a sníží počet chyb při rozšiřování aplikace.

## 2. Example

```python
from dataclasses import dataclass


@dataclass
class UserInput:
    name: str


class GreetingService:
    def make_message(self, data: UserInput) -> str:
        name = data.name.strip() or "světe"
        return f"Ahoj, {name}!"


# Pseudokód GUI callbacku:
# def on_button_click():
#     data = UserInput(name=input_widget.text())
#     output_widget.setText(service.make_message(data))
```

## 3. When to use

* Když GUI projekt roste a potřebujete ho udržovat dlouhodobě.
* Když chcete snížit riziko, že změna v UI rozbije business logiku.

## 4. Common mistakes

* ❌ Přímé volání databáze nebo API z každého widgetu – vzniká silné provázání vrstev.
* ❌ Blokující operace v UI vlákně – aplikace nereaguje na uživatele.

---

<details>
<summary>Optional: Deep dive</summary>

U větších aplikací se často používá MVC/MVP/MVVM styl. Nejde o dogma, ale o princip: UI má být tenké a orchestrace logiky má být mimo widgety.

</details>

---

**Další krok:** [Zpět na obsah](../README.md)
