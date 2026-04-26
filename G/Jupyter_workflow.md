# Jupyter workflow

Tato kapitola ukazuje, jak používat Jupyter notebooky tak, aby zůstaly reprodukovatelné a udržovatelné.

## 1. Concept

Jupyter je výborný pro exploraci dat, prototypy a sdílení analýz. Problém nastává, když notebook obsahuje skrytý stav a nejde spustit od začátku do konce.

Praktický workflow:

- malé, logické buňky
- průběžné markdown poznámky
- pravidelně spouštět Run All
- stabilní logiku přesouvat do `.py` modulů

## 2. Example

```python
import pandas as pd

# Cell 1: načtení dat
sales = pd.DataFrame({"month": ["Jan", "Feb", "Mar"], "value": [10, 12, 9]})

# Cell 2: transformace
sales["pct_change"] = sales["value"].pct_change().fillna(0)

# Cell 3: výstup
print(sales)
```

## 3. When to use

* Když analyzujete nový dataset a potřebujete rychle iterovat.
* Když vytváříte report nebo technickou demonstraci.
* Když experimentujete s feature engineeringem před produkční implementací.

## 4. Common mistakes

* ❌ Notebook funguje jen po ručním přeskakování buněk.
* ❌ Produkční logika zůstane jen v notebooku bez testovatelného modulu.
* ❌ Chybějící dokumentace, proč byl zvolen konkrétní postup.

---

<details>
<summary>Optional: Deep dive</summary>

V týmu se vyplatí definovat notebook standard: názvy souborů, pořadí sekcí, požadavek na Run All před commitem a oddělení explorace od produkčního kódu.

</details>

---

**Další krok:** [Kvalita dat a validace](Kvalita_dat.md)
