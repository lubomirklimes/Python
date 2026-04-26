# Serializace

Tato kapitola vysvětluje převod dat do přenositelné podoby pomocí `json` a upozorňuje na bezpečnostní rizika u `pickle`.

## 1. Concept

Serializace znamená převést objekt do formátu, který můžete uložit nebo poslat po síti. V Pythonu je výchozí bezpečná volba `json`, protože je čitelný, jazykově nezávislý a vhodný pro API i konfigurace.

`pickle` umí uložit i složité Python objekty, ale při načítání může spouštět kód. Proto nikdy nenačítejte pickle data z nedůvěryhodného zdroje.

## 2. Example

```python
import json
import pickle
from pathlib import Path

payload = {"name": "Karel", "score": 42, "active": True}

json_text = json.dumps(payload, ensure_ascii=False, indent=2)
print(json_text)

restored = json.loads(json_text)
print(restored["name"])

safe_path = Path("state.pkl")
safe_path.write_bytes(pickle.dumps(payload))
loaded = pickle.loads(safe_path.read_bytes())
print(loaded["score"])
```

## 3. When to use

* Když ukládáte jednoduchá data nebo komunikujete přes API, použijte `json`.
* Když potřebujete interně uložit Python objekty mezi běhy stejné aplikace, můžete použít `pickle` v důvěryhodném prostředí.

## 4. Common mistakes

* ❌ Použití `pickle.loads()` na data z internetu nebo od uživatele – vysoké bezpečnostní riziko.
* ❌ Uložení objektů bez verze schématu – po změně struktury dat může deserializace selhat.

---

<details>
<summary>Optional: Deep dive</summary>

Pro dlouhodobé ukládání dat je často lepší JSON + explicitní verze datového formátu. U kritických systémů je dobré zavést validaci vstupu při načítání, například přes `pydantic` nebo vlastní kontrolu polí.

</details>

---

**Další krok:** [Networking](Networking.md)
