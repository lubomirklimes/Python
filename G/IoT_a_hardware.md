# IoT a hardware

Tato kapitola popisuje použití Pythonu na Raspberry Pi a MicroPythonu na mikrokontrolérech.

## 1. Concept

Python je na Raspberry Pi velmi populární pro senzory, GPIO automatizaci a domácí projekty. U mikrokontrolérů (ESP32, RP2040) se často používá MicroPython, který je odlehčený a přizpůsobený omezenému hardware.

Python je vhodný pro prototypy a vzdělávání. Méně vhodný je tam, kde potřebujete velmi přísné realtime chování nebo extrémně nízkou spotřebu - tam často vítězí C/C++.

## 2. Example

```python
# Ukázka logiky čtení senzoru (simulace bez HW)
from dataclasses import dataclass


@dataclass
class SensorReading:
    temperature: float
    humidity: float


def evaluate(reading: SensorReading) -> str:
    if reading.temperature > 30:
        return "Zapnout chlazení"
    return "Stav v normě"


reading = SensorReading(temperature=28.5, humidity=45.0)
print(evaluate(reading))

# Na Raspberry Pi by následně šlo řídit GPIO, např. přes gpiozero.
```

## 3. When to use

* Když stavíte IoT prototyp, domácí automatizaci nebo měření ze senzorů.
* Když potřebujete rychle iterovat nad logikou bez složitého toolchainu.

## 4. Common mistakes

* ❌ Podcenění napájení, teploty a stability zařízení v reálném prostředí.
* ❌ Očekávání desktopového výkonu na mikrokontroléru.

---

<details>
<summary>Optional: Deep dive</summary>

U IoT systémů je zásadní observability: logy, heartbeat a vzdálené aktualizace. Bez toho je dlouhodobá správa zařízení v terénu velmi náročná.

</details>

---

**Další krok:** [Game development](Game_development.md)
