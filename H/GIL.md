# GIL (Global Interpreter Lock)

Tato kapitola objasňuje, co je GIL v CPythonu a jaké má praktické důsledky pro paralelismus.

## 1. Concept

GIL je zámek, který v jednom procesu CPythonu dovolí běh Python bytecode jen jednomu vláknu v daný okamžik. Díky tomu je jednodušší správa paměti a interních struktur interpreteru, ale zároveň to omezuje paralelní běh CPU-bound Python kódu ve více vláknech.

Důsledky:

- I/O-bound úlohy mohou z vláken těžit (čekání na síť/disk GIL často uvolní)
- CPU-bound úlohy ve vláknech škálují špatně
- pro CPU paralelismus je obvykle vhodnější `multiprocessing`

## 2. Example

```python
import threading
import time


def cpu_work(n: int) -> int:
    total = 0
    for i in range(n):
        total += i * i
    return total


N = 3_000_000

start = time.perf_counter()
cpu_work(N)
cpu_work(N)
seq = time.perf_counter() - start

start = time.perf_counter()
t1 = threading.Thread(target=cpu_work, args=(N,))
t2 = threading.Thread(target=cpu_work, args=(N,))
t1.start()
t2.start()
t1.join()
t2.join()
threads = time.perf_counter() - start

print(f"Sekvenčně: {seq:.3f}s")
print(f"2 vlákna (CPU-bound): {threads:.3f}s")
```

## 3. When to use

* Když rozhodujete mezi `threading`, `multiprocessing` a `asyncio`.
* Když ladíte, proč více vláken nezrychluje výpočetní část aplikace.

## 4. Common mistakes

* ❌ Očekávání lineárního zrychlení CPU výpočtu jen přidáním vláken.
* ❌ Zaměnění problému GIL s pomalým I/O nebo neefektivním algoritmem.

---

<details>
<summary>Optional: Deep dive</summary>

Některé nativní knihovny (např. části NumPy) GIL interně uvolňují, takže více vláken může dávat smysl i u numeriky. Vždy ale měřte konkrétní workload, ne obecné pravidlo.

</details>

---

**Další krok:** [Multithreading](Multithreading.md)
