# C extensions / Cython

Tato kapitola vysvětluje, jak obejít limity čistého Pythonu pomocí nativního kódu.

## 1. Concept

Když je úzké místo v CPU-heavy části, můžete kritickou část přesunout do C/C++ extension nebo použít Cython. Cython umožní psát kód podobný Pythonu a zkompilovat ho do C extension modulu.

Běžná strategie je držet většinu projektu v Pythonu a optimalizovat jen hot spoty. Tím získáte výkon bez ztráty čitelnosti celého kódu.

## 2. Example

```python
# app_math.py
# V produkci může být fast_sum implementované v Cythonu jako fast_sum_ext.

try:
    from fast_sum_ext import fast_sum  # kompilované rozšíření
except ImportError:
    def fast_sum(values: list[int]) -> int:
        total = 0
        for value in values:
            total += value
        return total


if __name__ == "__main__":
    print(fast_sum(list(range(1_000_000))))
```

## 3. When to use

* Když profilování ukáže malý počet CPU funkcí jako hlavní bottleneck.
* Když potřebujete vyšší výkon, ale nechcete přepisovat celý systém.

## 4. Common mistakes

* ❌ Přepis velké části projektu do Cythonu bez měření přínosu.
* ❌ Ignorování build/toolchain složitosti při nasazení na více platforem.

---

<details>
<summary>Optional: Deep dive</summary>

Vedle Cythonu lze zvažovat i Numba (JIT) nebo přímé C/C++ extension. Volba závisí na typu workloadu, požadavcích na deploy a zkušenostech týmu s nativním toolchainem.

</details>

---

**Další krok:** [Profiling](Profiling.md)
