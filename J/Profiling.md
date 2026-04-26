# Profiling

Tato kapitola ukazuje, jak měřit výkon přes `cProfile` a kdy použít `line_profiler`.

## 1. Concept

Profiling je základ výkonové práce. Místo odhadů vám ukáže, které funkce nebo řádky skutečně spotřebují čas. `cProfile` je ve standardní knihovně a je dobrý první krok. `line_profiler` jde hlouběji na úroveň jednotlivých řádků.

Doporučený postup:

- nejdřív získat hrubý přehled (`cProfile`)
- potom detailně měřit kritické funkce (`line_profiler`)
- po změně znovu měřit, nehádat

## 2. Example

```python
import cProfile
import pstats


def compute() -> int:
    total = 0
    for i in range(300_000):
        total += i * i
    return total


profiler = cProfile.Profile()
profiler.enable()
compute()
profiler.disable()

stats = pstats.Stats(profiler).sort_stats("cumulative")
stats.print_stats(5)

# line_profiler (externí nástroj) se typicky spouští přes kernprof:
# kernprof -l -v your_script.py
```

## 3. When to use

* Když chcete přesně určit, co zpomaluje aplikaci.
* Když ověřujete, že optimalizace opravdu přinesla zlepšení.

## 4. Common mistakes

* ❌ Optimalizace bez baseline měření před změnou.
* ❌ Vyvozování závěrů z jednorázového běhu bez opakování.

---

<details>
<summary>Optional: Deep dive</summary>

Profiling v developmentu i produkci má různé cíle. V produkci často sledujete metriky latence a throughput, zatímco lokálně detailně analyzujete konkrétní hot spoty. Obojí je potřeba kombinovat.

</details>

---

**Další krok:** [Zpět na obsah](../README.md)
