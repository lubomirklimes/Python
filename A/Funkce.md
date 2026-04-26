# Funkce

V této kapitole se naučíte, jak rozdělit program do znovupoužitelných funkcí s parametry a návratovými hodnotami.

## 1. Concept

Funkce je pojmenovaný blok kódu, který řeší konkrétní úkol. Definuje se pomocí `def`. Parametry předávají vstupní data, návratová hodnota (`return`) vrací výsledek zpět volajícímu.

U parametrů často využijete:

- výchozí hodnotu (`discount=0.0`)
- pojmenované argumenty při volání (`tax=0.21`)

Díky tomu je kód čitelnější a lépe se testuje.

## 2. Example

```python
def final_price(price: float, tax: float = 0.21, discount: float = 0.0) -> float:
    discounted = price * (1 - discount)
    return discounted * (1 + tax)

basic = final_price(1000)
promo = final_price(1000, discount=0.15)
custom = final_price(1000, tax=0.12, discount=0.10)

print(f"Zakladni cena: {basic:.2f}")
print(f"Akcni cena: {promo:.2f}")
print(f"Vlastni sazby: {custom:.2f}")
```

## 3. When to use

* Když se stejná logika opakuje na více místech.
* Když chcete oddělit výpočty od vstupu/výstupu a lépe je testovat.

## 4. Common mistakes

* ❌ Funkce pouze vypisuje výsledek místo `return` – hodnota pak nejde dál použít.
* ❌ Příliš mnoho odpovědností v jedné funkci – funkce je pak těžko čitelná i udržovatelná.

---

<details>
<summary>Optional: Deep dive</summary>

Dobrá funkce mívá jasný název, krátké tělo a jeden hlavní důvod ke změně. Typové anotace sice nejsou povinné, ale výrazně pomáhají při orientaci v projektu.

</details>

---

**Další krok:** [Kolekce](Kolekce.md)
