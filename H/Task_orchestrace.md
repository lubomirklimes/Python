# Task orchestrace

Tato kapitola ukazuje orchestraci async úloh přes `gather()` a `wait()`.

## 1. Concept

V `asyncio` nestačí jen vytvořit coroutines, je potřeba je správně orchestrvat: spouštět, čekat, rušit, řešit timeouty a chyby.

- `asyncio.gather()` vrátí výsledky v pořadí vstupních úloh
- `asyncio.wait()` dává jemnější kontrolu (např. `FIRST_COMPLETED`)

Správná orchestrace je klíčová pro robustní služby: při chybě musíte vědět, které úlohy doběhly, které běží dál a co zrušit.

## 2. Example

```python
import asyncio


async def job(name: str, delay: float) -> str:
    await asyncio.sleep(delay)
    return f"{name} done"


async def main() -> None:
    # gather: čeká na vše a vrací výsledky v pořadí argumentů
    results = await asyncio.gather(job("A", 0.4), job("B", 0.2))
    print("gather:", results)

    # wait: jemnější řízení, například první dokončená úloha
    tasks = {asyncio.create_task(job("X", 0.5)), asyncio.create_task(job("Y", 0.1))}
    done, pending = await asyncio.wait(tasks, return_when=asyncio.FIRST_COMPLETED)

    for task in done:
        print("wait done:", task.result())

    for task in pending:
        task.cancel()
    await asyncio.gather(*pending, return_exceptions=True)


if __name__ == "__main__":
    asyncio.run(main())
```

## 3. When to use

* Když koordinujete více síťových volání a řešíte timeout nebo fallback.
* Když potřebujete řídit životní cyklus většího počtu současných úloh.

## 4. Common mistakes

* ❌ Vytvořené tasky bez await/cancel - vznikají "ztracené" běžící úlohy.
* ❌ Ignorování chyb v taskách - selhání pak zůstane skryté.

---

<details>
<summary>Optional: Deep dive</summary>

V Pythonu 3.11 můžete použít `asyncio.TaskGroup`, která zjednodušuje structured concurrency. Zlepšuje to správu chyb i rušení potomků při selhání jedné úlohy.

</details>

---

**Další krok:** [I/O optimalizace](IO_optimalizace.md)
