# Generátory a iterátory

Tato kapitola vysvětluje rozdíl mezi iterátorem a generátorem a ukazuje praktické použití `yield`, `iter()` a `next()`.

## 1. Concept

Iterátor je objekt, který vrací hodnoty postupně a pamatuje si svůj stav. Generátor je pohodlný způsob, jak iterátor vytvořit pomocí funkce s `yield`.

Proč to existuje? Protože nemusíte držet všechna data v paměti najednou. Generátory vrací hodnoty "po kusech" (lazy evaluation), což je velmi užitečné pro velké soubory nebo streamy dat.

`iter(obj)` vytvoří iterátor a `next(it)` vrátí další položku. Když data dojdou, vyvolá se `StopIteration`.

## 2. Example

```python
def read_chunks(text: str, size: int):
    for start in range(0, len(text), size):
        yield text[start:start + size]

chunk_iter = iter(read_chunks("abcdefghijklmnopqrstuvwxyz", 5))

print(next(chunk_iter))  # abcde
print(next(chunk_iter))  # fghij

for chunk in chunk_iter:
    print(chunk)
```

## 3. When to use

* Když zpracováváte hodně dat a nechcete je načíst celé do paměti.
* Když chcete postupné zpracování (pipeline), kde data tečou krok za krokem.

## 4. Common mistakes

* ❌ Převod generátoru na `list()` bez potřeby – ztratí se paměťová výhoda.
* ❌ Volání `next()` bez ošetření konce iterace v nízkoúrovňovém kódu.

---

<details>
<summary>Optional: Deep dive</summary>

Generátory jsou základem mnoha Python knihoven pro zpracování dat. Často je kombinujete s `itertools`, které přidává efektivní nástroje pro spojování, filtrování a skládání iterátorů.

</details>

---

**Další krok:** [Typování v Pythonu](Typovani_v_Pythonu.md)
