# Práce s databázemi v cloudu

Tato kapitola pokrývá připojení Python aplikace k managed databázím, migrace a connection pooling.

## 1. Concept

V cloudu často používáte managed PostgreSQL/MySQL, kde provider řeší zálohy, patching a HA. Aplikace se připojuje přes connection string uložený v environment variable.

Klíčové body:

- migrace schématu (např. Alembic) drží DB konzistentní napříč prostředími
- bezpečné připojení používá TLS a tajné údaje mimo repozitář
- connection pooling omezuje režii vytváření spojení

## 2. Example

```python
import os
from sqlalchemy import create_engine, text

DATABASE_URL = os.getenv("DATABASE_URL")
if not DATABASE_URL:
    raise RuntimeError("Chybí DATABASE_URL")

# pool_pre_ping pomáhá ošetřit stale connection.
engine = create_engine(DATABASE_URL, pool_size=5, max_overflow=10, pool_pre_ping=True)

with engine.connect() as conn:
    result = conn.execute(text("SELECT 1 AS ok"))
    print(result.scalar_one())

print("Migrace by se spouštěly v CI/CD kroku (např. alembic upgrade head).")
```

## 3. When to use

* Když aplikace běží v cloudu a potřebuje spolehlivé perzistentní úložiště.
* Když chcete oddělit životní cyklus aplikace od databázové infrastruktury.
* Když potřebujete škálovat čtení/zápis a mít řízené migrace.

## 4. Common mistakes

* ❌ Heslo v URL přímo v kódu nebo v README.
* ❌ Každý request otevírá nové DB spojení bez poolingu.
* ❌ Nasazení aplikace bez spuštění migrací pro cílové prostředí.

---

<details>
<summary>Optional: Deep dive</summary>

Pool sizing závisí na typu workloadu a limitu DB. Příliš velký pool může databázi přetížit, příliš malý zvyšuje latenci. Měřte čekání na connection a latency query, ne jen CPU aplikace.

</details>

---

**Další krok:** [Messaging a fronty](Messaging.md)
