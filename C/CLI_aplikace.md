# CLI aplikace

V této kapitole si ukážeme, jak vytvořit jednoduchou příkazovou aplikaci pomocí `argparse`.

## 1. Concept

`argparse` je standardní způsob, jak zpracovat argumenty příkazové řádky. Pomáhá definovat povinné i volitelné parametry, automaticky generuje `--help` a validuje vstupy.

Pro malé i střední nástroje je to bezpečná výchozí volba, protože nemusíte psát vlastní parser argumentů.

## 2. Example

```python
import argparse
from pathlib import Path

parser = argparse.ArgumentParser(description="Jednoduchy pocitac slov v souboru")
parser.add_argument("file", type=Path, help="Cesta k textovemu souboru")
parser.add_argument("--min-length", type=int, default=1, help="Minimalni delka slova")
args = parser.parse_args()

text = args.file.read_text(encoding="utf-8")
words = [w for w in text.split() if len(w) >= args.min_length]

print(f"Pocet slov: {len(words)}")
```

## 3. When to use

* Když vytváříte interní nástroje pro automatizaci nebo build/deploy skripty.
* Když potřebujete předávat parametry uživatele přehledně a robustně.

## 4. Common mistakes

* ❌ Ruční parsování `sys.argv` bez validace – rychle vznikají chyby a nejasné hlášky.
* ❌ Chybějící nápověda (`help`) u argumentů – uživatel neví, jak nástroj použít.

---

<details>
<summary>Optional: Deep dive</summary>

Jakmile CLI nástroj roste, přidejte subcommands (`parser.add_subparsers`) a oddělte logiku do samostatných funkcí. Zachováte tím čitelnost i testovatelnost.

</details>

---

**Další krok:** [Zpět na obsah](../README.md)
