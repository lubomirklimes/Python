# I/O optimalizace

Tato kapitola se zaměřuje na zrychlení práce se soubory, sítí a databázemi, kde bývá úzké místo právě I/O.

## 1. Concept

U I/O-bound aplikací výkon často nezlepšuje rychlejší CPU kód, ale lepší práce s čekáním. Typické techniky jsou batching, buffering, connection pooling, paralelní I/O a omezení zbytečných round-tripů.

Praktický postup:

- měřit latenci jednotlivých kroků
- snížit počet I/O operací (např. větší dávky)
- provádět čekání souběžně (threads/async)

## 2. Example

```python
from pathlib import Path
import time

path = Path("big.txt")
path.write_text("\n".join(str(i) for i in range(200_000)), encoding="utf-8")

start = time.perf_counter()
with path.open("r", encoding="utf-8") as f:
    data = f.read()
full_read_time = time.perf_counter() - start

start = time.perf_counter()
count = 0
with path.open("r", encoding="utf-8") as f:
    for line in f:
        if line:
            count += 1
line_read_time = time.perf_counter() - start

print(f"read(): {full_read_time:.4f}s")
print(f"line-by-line: {line_read_time:.4f}s, lines={count}")
```

## 3. When to use

* Když aplikace tráví většinu času čekáním na disk, síť nebo databázi.
* Když potřebujete zvýšit throughput bez zásadní změny byznys logiky.

## 4. Common mistakes

* ❌ Optimalizace CPU části, i když bottleneck je síť nebo disk.
* ❌ Příliš jemné I/O operace (mnoho malých requestů/čtení) místo batch přístupu.

---

<details>
<summary>Optional: Deep dive</summary>

I/O optimalizace často souvisí s architekturou: cache vrstvy, fronty, asynchronní zpracování a správné timeouty. Bez měření p95/p99 latencí je těžké rozhodnout, která změna má reálný dopad.

</details>

---

**Další krok:** [C extensions / Cython](C_extensions_Cython.md)
