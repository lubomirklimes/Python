# CLI tools

Tato kapitola ukazuje, jak tvořit robustní nástroje příkazové řádky v Pythonu.

## 1. Concept

CLI nástroj je často nejrychlejší cesta, jak dodat užitečnou automatizaci týmu. Python je vhodný díky čitelnosti, knihovnám a snadnému balení. Typicky stavíte příkazy jako `init`, `run`, `report` a předáváte parametry přes argumenty.

Python je výborný pro interní a integrační nástroje. Méně vhodný může být pro extrémně malý binární footprint bez interpreteru.

## 2. Example

```python
import argparse
from pathlib import Path


def main() -> None:
    parser = argparse.ArgumentParser(description="Vypis souboru podle pripony")
    parser.add_argument("path", type=Path)
    parser.add_argument("--suffix", default=".py")
    args = parser.parse_args()

    files = sorted(p for p in args.path.rglob(f"*{args.suffix}") if p.is_file())
    for file in files:
        print(file)


if __name__ == "__main__":
    main()
```

## 3. When to use

* Když potřebujete interní nástroj pro developery, QA nebo DevOps workflow.
* Když chcete snadno skriptovatelný nástroj použitelný v CI/CD.

## 4. Common mistakes

* ❌ Chybějící návratové kódy a jasné chybové zprávy.
* ❌ Nesrozumitelná nápověda parametrů - nástroj je pak těžko použitelný.

---

<details>
<summary>Optional: Deep dive</summary>

Pro větší CLI se hodí command framework (např. Typer, Click) a pluginový model. Udržujte ale jednoduché veřejné rozhraní příkazů, aby nástroj šel snadno adoptovat.

</details>

---

**Další krok:** [Zpět na obsah](../README.md)
