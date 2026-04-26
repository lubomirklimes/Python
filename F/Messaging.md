# Messaging a fronty

Tato kapitola vysvětluje producer/consumer model a proč se fronty používají místo přímého HTTP volání.

## 1. Concept

Fronta oddělí producenta událostí od konzumenta. Producer vloží zprávu a nemusí čekat na dokončení celé práce. Consumer ji zpracuje asynchronně. V praxi to zvyšuje odolnost a umožňuje vyrovnat špičky.

Důležité principy:

- retry: dočasné chyby se opakují
- dead-letter queue: trvale chybné zprávy se odloží mimo hlavní frontu
- idempotence: opakované zpracování stejné zprávy nesmí rozbít data

## 2. Example

```python
from queue import Queue, Empty
import time

messages: Queue[dict] = Queue()
processed_ids: set[str] = set()

# Producer
messages.put({"id": "evt-1", "payload": {"email": "user@example.com"}})

# Consumer
while True:
    try:
        msg = messages.get(timeout=0.5)
    except Empty:
        break

    msg_id = msg["id"]
    if msg_id in processed_ids:
        print("Idempotent skip", msg_id)
        continue

    try:
        # Simulace zpracování
        time.sleep(0.1)
        processed_ids.add(msg_id)
        print("Processed", msg_id)
    except Exception:
        # Retry by v praxi řešila broker politika nebo consumer logika.
        messages.put(msg)
```

## 3. When to use

* Když potřebujete oddělit rychlou API odpověď od pomalejšího background zpracování.
* Když řešíte integrační scénáře mezi více službami.
* Když chcete zvládnout burst provoz bez pádu systému.

## 4. Common mistakes

* ❌ Chybějící idempotence při retry - vznikají duplicitní vedlejší efekty.
* ❌ Fronta bez dead-letter strategie - vadné zprávy blokují systém.
* ❌ Použití fronty i tam, kde stačí jednoduché sync HTTP volání.

---

<details>
<summary>Optional: Deep dive</summary>

Technologie jako RabbitMQ, Azure Service Bus nebo cloud queue služby řeší doručování odlišně. Návrh zpráv (schema versioning, correlation ID) je často důležitější než volba konkrétního brokeru.

</details>

---

**Další krok:** [Konfigurace a secrets](Configuration_secrets.md)
