# Multithreading

Tato kapitola ukazuje použití modulu `threading` a vysvětluje, kdy vlákna dávají v Pythonu smysl.

## 1. Concept

Multithreading je vhodný hlavně pro I/O-bound úlohy: síťové volání, čekání na disk, API requesty nebo fronty úloh. Vlákna sdílí paměť procesu, takže komunikace je snadná, ale je potřeba hlídat synchronizaci (`Lock`, `Queue`, `Event`).

Kvůli GIL vlákna obvykle nezrychlují čistě CPU-bound Python výpočty.

## 2. Example

```python
import threading
import time
from queue import Queue


def worker(name: str, out: Queue[str]) -> None:
    # Simulace I/O čekání
    time.sleep(0.4)
    out.put(f"{name} hotovo")


result_queue: Queue[str] = Queue()
threads = [threading.Thread(target=worker, args=(f"t{i}", result_queue)) for i in range(4)]

start = time.perf_counter()
for t in threads:
    t.start()
for t in threads:
    t.join()

elapsed = time.perf_counter() - start
results = [result_queue.get() for _ in threads]

print(results)
print(f"Celkový čas: {elapsed:.3f}s")
```

## 3. When to use

* Když máte více nezávislých I/O operací a chcete snížit čekání.
* Když potřebujete jednoduchý paralelní model ve sdílené paměti.

## 4. Common mistakes

* ❌ Sdílení mutable dat bez synchronizace - vznikají race conditions.
* ❌ Použití vláken na CPU-heavy kód s očekáváním výrazného zrychlení.

---

<details>
<summary>Optional: Deep dive</summary>

Na vyšší úrovni se často používá `concurrent.futures.ThreadPoolExecutor`, který zjednoduší plánování práce i sběr výsledků. Pro robustnost je důležité řešit timeouty a rušení úloh.

</details>

---

**Další krok:** [Multiprocessing](Multiprocessing.md)
