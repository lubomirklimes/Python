# Práce se soubory a OS

Tato kapitola ukazuje, jak používat moduly `os` a `pathlib` pro práci se soubory, složkami a cestami napříč operačními systémy.

## 1. Concept

Při práci s cestami je dnes nejlepší výchozí volba `pathlib`. Nabízí objektový přístup (`Path`) a přehlednější kód než ruční skládání řetězců. Modul `os` je stále užitečný pro operace nad prostředím (proměnné prostředí, procesní informace) a některé systémové funkce.

Bezpečný základ je:

- používat `Path` místo ručního spojování cest přes `"/"` nebo `"\\"`
- před čtením souboru ověřit, že existuje
- pracovat s explicitním kódováním `utf-8`

## 2. Example

```python
from pathlib import Path
import os

base = Path.cwd()
notes_dir = base / "data"
notes_file = notes_dir / "notes.txt"

notes_dir.mkdir(exist_ok=True)

if "APP_ENV" not in os.environ:
    os.environ["APP_ENV"] = "dev"

notes_file.write_text("Ahoj z pathlib!\n", encoding="utf-8")

if notes_file.exists():
    content = notes_file.read_text(encoding="utf-8")
    print(content.strip())

print(f"Prostredi: {os.environ['APP_ENV']}")
```

## 3. When to use

* Když píšete skript, který vytváří adresáře, čte konfiguraci nebo generuje výstupy do souborů.
* Když chcete mít stejný kód pro Windows, Linux i macOS bez ručního řešení oddělovačů cest.

## 4. Common mistakes

* ❌ Skládání cesty jako textu (`"folder/" + name`) – hrozí chyby mezi platformami.
* ❌ Předpoklad, že soubor existuje – při čtení bez kontroly často dojde k `FileNotFoundError`.

---

<details>
<summary>Optional: Deep dive</summary>

`Path.resolve()` je užitečné pro získání absolutní normalizované cesty. U větších nástrojů se vyplatí oddělit cesty pro vstupy, výstupy a dočasná data, aby byl provoz předvídatelný a snadno testovatelný.

</details>

---

**Další krok:** [Datum a čas](Datum_a_cas.md)
