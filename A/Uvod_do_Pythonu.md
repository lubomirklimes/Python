# Úvod do Pythonu

Tato kapitola vysvětluje, co je Python, jak ho nainstalovat a jak spustit první skript.

## 1. Concept

Python je univerzální programovací jazyk, který je čitelný a vhodný pro začátečníky i profesionály. Používá se ve webovém vývoji, automatizaci, datové analýze, testování, DevOps i v AI. Velká výhoda je bohatý ekosystém knihoven a jednoduchý zápis kódu.

Pro běžnou práci potřebujete tři věci: Python, nástroj `pip` pro instalaci balíčků a izolované prostředí (`virtualenv` přes `venv`). Izolované prostředí oddělí závislosti projektu od zbytku systému, takže si projekty navzájem „nerozbijí“ verze knihoven.

## 2. Example

```python
# first_script.py
message = "Ahoj, svete Pythonu!"
print(message)
```

Typický postup v terminálu (Windows PowerShell):

```powershell
py -3.11 --version
py -m pip --version
py -m venv .venv
.\.venv\Scripts\Activate.ps1
python first_script.py
```

## 3. When to use

* Když chcete rychle napsat skript na automatizaci opakované práce.
* Když začínáte programovat a potřebujete jazyk s jednoduchou syntaxí.

## 4. Common mistakes

* ❌ Instalace balíčků globálně místo do virtuálního prostředí – projekt pak není snadno přenositelný.
* ❌ Míchání různých Python instalací – `python` a `pip` pak mohou ukazovat na jinou verzi.

---

<details>
<summary>Optional: Deep dive</summary>

Nejběžnější implementace je CPython. V praxi to znamená, že když dokumentace mluví o Pythonu 3.11+, typicky jde právě o CPython. Existují i jiné implementace (např. PyPy), ale pro začátek je bezpečné držet se CPythonu.

</details>

---

**Další krok:** [Základní syntaxe](Zakladni_syntaxe.md)
