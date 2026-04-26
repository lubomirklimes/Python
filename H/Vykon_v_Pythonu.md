# Výkon v Pythonu

Tato kapitola vysvětluje, proč je Python často pomalejší než kompilované jazyky, a jak volit praktické optimalizační strategie.

## 1. Concept

Python bývá pomalejší hlavně kvůli interpretaci bytecode, dynamickému typování a režii práce s objekty. To ale neznamená, že je vždy "pomalý" v praxi. U mnoha aplikací je dominantní I/O (disk, síť, databáze), kde CPU není hlavní úzké místo.

Rozumná strategie je:

- nejdřív zlepšit algoritmus a datové struktury
- používat vestavěné funkce a knihovny psané v C (např. NumPy)
- optimalizovat až po měření

## 2. Example

```python
import timeit

numbers = list(range(100_000))

loop_time = timeit.timeit(
    "result=[]\nfor n in numbers:\n    result.append(n*n)",
    number=20,
    globals={"numbers": numbers},
)

comp_time = timeit.timeit(
    "result=[n*n for n in numbers]",
    number=20,
    globals={"numbers": numbers},
)

print(f"for-loop: {loop_time:.4f}s")
print(f"list comprehension: {comp_time:.4f}s")
```

## 3. When to use

* Když máte reálný problém s latencí nebo throughput a potřebujete ho řešit systematicky.
* Když chcete škálovat datové zpracování bez okamžitého přepisu do jiného jazyka.

## 4. Common mistakes

* ❌ Předčasná optimalizace bez profilingu - můžete ztratit čas na nesprávném místě.
* ❌ Mikrooptimalizace při špatné algoritmické složitosti - hlavní problém zůstane.

---

<details>
<summary>Optional: Deep dive</summary>

Výkon se dá zjednodušeně popsat jako vztah mezi CPU, pamětí a I/O. I když je Python interpreter pomalejší na čistý CPU výpočet, celkové řešení může být rychlé díky efektivnímu návrhu, cache, batchingu a využití nativních knihoven.

</details>

---

**Další krok:** [GIL (Global Interpreter Lock)](GIL.md)
