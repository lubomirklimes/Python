# Feature engineering

Tato kapitola ukazuje, jak z původních dat vytvářet lepší vstupní proměnné (features).

## 1. Concept

Feature engineering je proces, kdy z raw dat vytváříte reprezentaci, která modelu usnadní učení. Často jde o nejdůležitější část ML projektu, protože kvalita vstupních feature výrazně ovlivní výsledek.

Typické techniky:

- odvozené sloupce (poměry, rozdíly, interakce)
- práce s datem a časem (den v týdnu, měsíc, sezóna)
- kódování kategorií (one-hot)
- škálování numerických dat

## 2. Example

```python
import pandas as pd

orders = pd.DataFrame(
    {
        "order_total": [1200, 800, 1500],
        "items_count": [3, 1, 4],
        "order_date": ["2026-01-10", "2026-02-14", "2026-03-01"],
        "channel": ["web", "mobile", "web"],
    }
)

orders["order_date"] = pd.to_datetime(orders["order_date"])
orders["avg_item_price"] = orders["order_total"] / orders["items_count"]
orders["weekday"] = orders["order_date"].dt.dayofweek

encoded = pd.get_dummies(orders, columns=["channel"], dtype=int)
print(encoded)
```

## 3. When to use

* Když model s raw daty nedává dostatečně dobré výsledky.
* Když potřebujete zachytit doménové vztahy, které model sám nevidí.
* Když připravujete stabilní feature set pro produkční scoring.

## 4. Common mistakes

* ❌ Feature vytvořená z budoucí informace (target leakage).
* ❌ Přehnaně složité feature bez ověření přínosu.
* ❌ Nekonzistentní výpočet feature mezi tréninkem a produkcí.

---

<details>
<summary>Optional: Deep dive</summary>

Dobré features jsou často doménově specifické. Spolupráce s byznys expertem bývá cennější než slepé zkoušení velkého množství obecných transformací.

</details>

---

**Další krok:** [Data pipeline - načtení, čištění a transformace dat](Data_pipeline.md)
