# Datové formáty a ukládání dat

Tato kapitola porovnává běžné datové formáty a jejich použití v datové praxi.

## 1. Concept

Volba formátu ovlivňuje výkon, čitelnost i interoperabilitu. Nejčastější formáty:

- CSV: jednoduchý, čitelný, ale bez typové informace
- JSON: vhodný pro API a stromová data
- Parquet: sloupcový formát, efektivní pro analytiku
- Pickle: Python-specifický, interní použití (ne pro nedůvěryhodná data)

Obecně platí: pro analytiku preferujte Parquet, pro interoperabilitu CSV/JSON.

## 2. Example

```python
import pandas as pd
from pathlib import Path

path = Path("data")
path.mkdir(exist_ok=True)

df = pd.DataFrame({"id": [1, 2], "name": ["Anna", "Petr"], "score": [0.91, 0.78]})

df.to_csv(path / "sample.csv", index=False)
df.to_json(path / "sample.json", orient="records", force_ascii=False)

# Parquet vyžaduje pyarrow nebo fastparquet.
# df.to_parquet(path / "sample.parquet", index=False)

print("Uloženo CSV a JSON")
```

## 3. When to use

* Když navrhujete datové úložiště mezi kroky pipeline.
* Když sdílíte data mezi různými nástroji a týmy.
* Když řešíte kompromis mezi čitelností a výkonem.

## 4. Common mistakes

* ❌ Použití CSV pro velmi velké analytické datasety bez komprese.
* ❌ Uložení čísel jako textu bez validace schématu.
* ❌ Přenos pickle souborů mezi nedůvěryhodnými prostředími.

---

<details>
<summary>Optional: Deep dive</summary>

Kromě formátu je důležité i partitioning a naming konvence souborů. U velkých datových sad to významně ovlivní rychlost čtení a provozní náklady.

</details>

---

**Další krok:** [Jupyter workflow](Jupyter_workflow.md)
