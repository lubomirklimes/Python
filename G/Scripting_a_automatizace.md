# Scripting a automatizace

Tato kapitola ukazuje, jak Python použít pro automatizaci opakovaných úloh nad soubory a dávkovými procesy.

## 1. Concept

Python je výborný pro skripty, které propojí práci se soubory, parsování dat a volání externích příkazů. Umožní vám nahradit ruční kroky opakovatelným procesem. Typický případ je dávkové přejmenování souborů, převody formátů nebo reporty.

Python je vhodný, když potřebujete čitelnost a rychlý vývoj. Méně vhodný je pro velmi nízkoúrovňové operace s extrémními výkonovými nároky.

## 2. Example

```python
from pathlib import Path

source = Path("input")
source.mkdir(exist_ok=True)

for i in range(3):
    (source / f"report_{i}.txt").write_text(f"Soubor {i}\n", encoding="utf-8")

for file in source.glob("report_*.txt"):
    target = file.with_name(file.stem.upper() + file.suffix)
    file.rename(target)

print("Batch rename dokoncen")
```

## 3. When to use

* Když chcete automatizovat rutinní úlohy v týmu nebo CI prostředí.
* Když potřebujete rychle vytvořit interní nástroj bez složité infrastruktury.

## 4. Common mistakes

* ❌ Skript bez logování a bez chybových hlášek - při problému je těžké zjistit příčinu.
* ❌ Spouštění destruktivních operací bez dry-run režimu.

---

<details>
<summary>Optional: Deep dive</summary>

U robustnější automatizace se vyplatí přidat parametr `--dry-run`, aby šlo bezpečně ověřit dopad změn. Dále pomáhá idempotence: opakované spuštění by nemělo poškodit data.

</details>

---

**Další krok:** [Web scraping](Web_scraping.md)
