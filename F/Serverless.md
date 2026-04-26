# Serverless v Pythonu

Tato kapitola vysvětluje serverless model a jeho použití v Pythonu.

## 1. Concept

Serverless znamená, že nasazujete funkci nebo kontejnerový handler a platforma řeší většinu infrastruktury. Typické služby jsou AWS Lambda nebo Azure Functions. Platíte primárně za vykonání a čas běhu.

Výhody:

- rychlé nasazení event-driven scénářů
- automatické škálování
- nízká provozní režie

Trade-offy:

- cold start latence
- limity běhu a prostředí
- složitější lokální debugging některých scénářů

## 2. Example

```python
import json

# Koncept handleru ve stylu AWS Lambda.
def handler(event: dict, _context) -> dict:
    name = event.get("name", "světe")
    return {
        "statusCode": 200,
        "headers": {"Content-Type": "application/json"},
        "body": json.dumps({"message": f"Ahoj, {name}!"}, ensure_ascii=False),
    }


if __name__ == "__main__":
    print(handler({"name": "Petr"}, None))
```

## 3. When to use

* Když zpracováváte události (upload souboru, fronta, webhook) bez trvalého serveru.
* Když potřebujete rychle doručit menší backend funkcionalitu.
* Když je provoz nepravidelný a chcete platit podle skutečného využití.

## 4. Common mistakes

* ❌ Ignorování cold startu u latence citlivých endpointů.
* ❌ Těžký monolitický handler s dlouhým během.
* ❌ Chybějící observability, retry a idempotence u event-driven toku.

---

<details>
<summary>Optional: Deep dive</summary>

Serverless není jen o funkcích, ale o architektuře. Dobře funguje tam, kde jsou malé, nezávislé události. U dlouhých CPU úloh nebo stateful workflow bývá vhodnější jiný runtime.

</details>

---

**Další krok:** [Distribuované systémy - základní principy](Distribuovane_systemy.md)
