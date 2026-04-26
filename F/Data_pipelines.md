# Data pipelines

Tato kapitola vysvětluje, jak navrhovat datové pipeline od ingestu po validaci a publikaci výsledků.

## 1. Concept

Data pipeline je sled kroků, které data načtou, vyčistí, transformují a uloží do cílového systému. Cílem je opakovatelnost, spolehlivost a auditovatelnost. V praxi se pipeline často plánují dávkově (batch) nebo běží průběžně (stream).

Důležité zásady:

- krokům dejte jasné vstupy a výstupy
- logujte běhy a chybové stavy
- validujte kvalitu dat před publikací výsledku

## 2. Example

```python
from dataclasses import dataclass


@dataclass
class Record:
    id: int
    amount: float


def extract() -> list[dict]:
    return [{"id": 1, "amount": 10.5}, {"id": 2, "amount": 8.0}]


def transform(raw: list[dict]) -> list[Record]:
    return [Record(id=int(item["id"]), amount=float(item["amount"])) for item in raw]


def load(records: list[Record]) -> None:
    total = sum(r.amount for r in records)
    print(f"Ulozeno {len(records)} zaznamu, total={total}")


raw_data = extract()
records = transform(raw_data)
load(records)
```

## 3. When to use

* Když pravidelně zpracováváte data z více zdrojů do reportingu nebo ML.
* Když potřebujete kontrolovat kvalitu dat a dohledat, kde vznikla chyba.

## 4. Common mistakes

* ❌ Chybějící idempotence pipeline - opakované spuštění vytvoří nekonzistentní data.
* ❌ Slabý monitoring a alerting - problém zjistíte až pozdě.

---

<details>
<summary>Optional: Deep dive</summary>

Pro orchestraci se často používají nástroje jako Airflow nebo Prefect. I u malých pipeline se vyplatí oddělit orchestrace od transformační logiky a psát testy nad kritickými transformacemi.

</details>

---

**Další krok:** [Zpět na obsah](../README.md)
