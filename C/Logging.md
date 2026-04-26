# Logging

Tato kapitola vysvětluje, jak používat modul `logging` místo `print()` pro diagnostiku a provozní záznamy.

## 1. Concept

`logging` umožňuje zapisovat zprávy podle úrovní (`DEBUG`, `INFO`, `WARNING`, `ERROR`, `CRITICAL`) a směrovat je do konzole, souboru nebo jiného cíle. Díky tomu získáte kontrolu nad tím, co se loguje v dev/prod prostředí.

Dobrá výchozí praxe:

- používat `logger = logging.getLogger(__name__)`
- konfigurovat formát a úroveň na jednom místě
- nelogovat hesla, tokeny ani citlivá data

## 2. Example

```python
import logging

logging.basicConfig(
    level=logging.INFO,
    format="%(asctime)s %(levelname)s %(name)s: %(message)s",
)

logger = logging.getLogger(__name__)

logger.debug("Tato zprava je videt jen pri DEBUG")
logger.info("Aplikace startuje")

try:
    value = 10 / 0
    print(value)
except ZeroDivisionError:
    logger.exception("Nepodarilo se vypocitat hodnotu")
```

## 3. When to use

* Když potřebujete sledovat chování aplikace v produkci i při debugování.
* Když chcete mít auditní stopu chyb, warningů a důležitých událostí.

## 4. Common mistakes

* ❌ Použití `print()` místo logování v serverové aplikaci – chybí úrovně i struktura.
* ❌ Logování citlivých dat – riziko úniku informací do logů.

---

<details>
<summary>Optional: Deep dive</summary>

U větších aplikací se hodí oddělit loggery podle modulů, přidat rotaci souborů (`RotatingFileHandler`) a případně strukturované logy (JSON) pro snadnější analýzu v observability nástrojích.

</details>

---

**Další krok:** [CLI aplikace](CLI_aplikace.md)
