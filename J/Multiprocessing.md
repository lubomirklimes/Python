# Multiprocessing

Tato kapitola vysvětluje `multiprocessing` pro skutečný paralelní běh CPU-bound úloh.

## 1. Concept

`multiprocessing` spouští více procesů, každý má vlastní Python interpreter i vlastní GIL. Proto je to standardní cesta, jak v CPythonu využít více jader pro výpočetně náročné úlohy.

Trade-off je vyšší režie: start procesu, serializace dat mezi procesy a vyšší nároky na paměť.

## 2. Example

```python
from multiprocessing import Pool


def heavy(n: int) -> int:
    total = 0
    for i in range(n):
        total += i * i
    return total


if __name__ == "__main__":
    values = [3_000_000, 3_200_000, 3_400_000, 3_600_000]
    with Pool(processes=4) as pool:
        results = pool.map(heavy, values)
    print(results)
```

## 3. When to use

* Když je workload CPU-bound a chcete využít více jader.
* Když můžete práci rozdělit na nezávislé dávky.

## 4. Common mistakes

* ❌ Chybějící ochrana `if __name__ == "__main__":` (hlavně na Windows).
* ❌ Posílání obrovských objektů mezi procesy - komunikace pak přebije zisk výkonu.

---

<details>
<summary>Optional: Deep dive</summary>

U větších úloh často pomůže kombinace: data držet lokálně v procesu co nejdéle a mezi procesy přenášet jen nezbytné minimum. Pro sdílení stavu používejte jen tam, kde je to nutné.

</details>

---

**Další krok:** [Async programming](Async_programming.md)
