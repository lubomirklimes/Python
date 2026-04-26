# Observability: logy, metriky a tracing

Tato kapitola vysvětluje, jak monitorovat Python aplikaci pomocí logů, metrik a tracingu.

## 1. Concept

Observability znamená, že z produkce umíte rychle zjistit, co se děje a proč. Základní pilíře:

- logy: konkrétní události a chyby
- metriky: agregované hodnoty v čase (latence, error rate, throughput)
- tracing: průchod requestu přes více služeb

`print` nestačí, protože neobsahuje úrovně, kontext ani jednotný formát. Praktické minimum je structured logging s correlation ID.

## 2. Example

```python
import json
import logging
import uuid
from datetime import datetime, timezone

logger = logging.getLogger("app")
logger.setLevel(logging.INFO)
handler = logging.StreamHandler()
logger.addHandler(handler)

correlation_id = str(uuid.uuid4())

payload = {
    "ts": datetime.now(timezone.utc).isoformat(),
    "level": "INFO",
    "event": "order_created",
    "correlation_id": correlation_id,
    "order_id": 123,
}

logger.info(json.dumps(payload, ensure_ascii=False))
```

## 3. When to use

* Když provozujete API nebo worker služby v produkci.
* Když potřebujete rychle diagnostikovat incident a dopad na uživatele.
* Když stavíte distribuovaný systém s více komponentami.

## 4. Common mistakes

* ❌ Logy bez kontextu (request id, user id, service name).
* ❌ Jen technické metriky bez business metrik.
* ❌ Chybějící tracing napříč službami - problém se těžko lokalizuje.

---

<details>
<summary>Optional: Deep dive</summary>

Kombinace structured logs + metrics + distributed tracing je silnější než izolované nástroje. Důležité je sjednotit naming metrik a log schema, jinak observability data rychle degradují.

</details>

---

**Další krok:** [Serverless v Pythonu](Serverless.md)
