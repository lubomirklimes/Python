# Datum a čas

V této kapitole si ukážeme praktickou práci s datem a časem pomocí `datetime` a kdy použít modul `time`.

## 1. Concept

`datetime` je hlavní nástroj pro práci s daty, časem a formátováním. Modul `time` se hodí spíš pro jednoduché měření času nebo pauzy v programu. V běžných aplikacích je bezpečný výchozí přístup ukládat čas v UTC a převod na lokální čas řešit až na hranici aplikace (např. v UI).

Doporučení:

- používat timezone-aware hodnoty (`datetime.now(timezone.utc)`)
- pro trvání používat rozdíl dvou dat (`timedelta`) nebo monotónní čas

## 2. Example

```python
from datetime import datetime, timezone
import time

start = time.perf_counter()

created_at = datetime.now(timezone.utc)
print(f"Vytvoreno (UTC): {created_at.isoformat()}")

# Simulace krátké práce
time.sleep(0.2)

elapsed = time.perf_counter() - start
print(f"Trvani: {elapsed:.3f} s")

parsed = datetime.fromisoformat("2026-04-26T10:30:00+00:00")
print(f"Nactene datum: {parsed:%d.%m.%Y %H:%M:%S %Z}")
```

## 3. When to use

* Když ukládáte čas událostí (logy, audit, plánování úloh).
* Když potřebujete měřit délku běhu části kódu nebo timeouty.

## 4. Common mistakes

* ❌ Kombinace naive a timezone-aware `datetime` objektů – porovnávání pak selže.
* ❌ Použití `time.time()` pro přesné benchmarky – vhodnější je `time.perf_counter()`.

---

<details>
<summary>Optional: Deep dive</summary>

Pokud aplikace pracuje s více časovými pásmy, vyplatí se interně držet UTC a lokální čas řešit až při prezentaci. Snižuje to chyby kolem letního času.

</details>

---

**Další krok:** [Serializace](Serializace.md)
