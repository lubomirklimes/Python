# Async programming

Tato kapitola představuje `async/await` a modul `asyncio` pro efektivní I/O concurrency.

## 1. Concept

Asynchronní programování umožňuje obsluhovat mnoho I/O operací v jednom vlákně pomocí event loop. Když coroutine čeká na síť nebo disk, event loop mezitím spustí jinou práci.

Async se hodí pro I/O-bound scénáře s velkým počtem současných úloh. Není to náhrada za multiprocessing u čistě CPU-bound výpočtů.

## 2. Example

```python
import asyncio
import time


async def fake_call(name: str, delay: float) -> str:
    await asyncio.sleep(delay)
    return f"{name}: hotovo"


async def main() -> None:
    start = time.perf_counter()
    results = await asyncio.gather(
        fake_call("A", 0.5),
        fake_call("B", 0.7),
        fake_call("C", 0.3),
    )
    print(results)
    print(f"Celkový čas: {time.perf_counter() - start:.3f}s")


if __name__ == "__main__":
    asyncio.run(main())
```

## 3. When to use

* Když stavíte síťové služby, klienty API nebo realtime backendy s mnoha spojeními.
* Když potřebujete vysoký throughput I/O operací s nízkou režijní cenou vláken.

## 4. Common mistakes

* ❌ Blokující volání (např. `time.sleep`) uvnitř async kódu - zastaví celý event loop.
* ❌ Smíchání sync/async vrstev bez jasné hranice.

---

<details>
<summary>Optional: Deep dive</summary>

Pro CPU-heavy část v async aplikaci použijte `run_in_executor` nebo oddělený proces. Dobrá architektura jasně oddělí async I/O cestu a výpočetní část.

</details>

---

**Další krok:** [Task orchestrace](Task_orchestrace.md)
