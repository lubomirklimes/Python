# Distribuované systémy - základní principy

Tato kapitola shrnuje klíčové principy distribuovaných systémů a jejich dopad na Python aplikace.

## 1. Concept

Distribuovaný systém je síť spolupracujících komponent, které mohou selhat nezávisle na sobě. Oproti lokální aplikaci musíte počítat s latencí, výpadky sítě a neúplnými odpověďmi.

Základní principy:

- timeout: nikdy nečekat neomezeně
- retry: opakovat jen bezpečně a řízeně
- idempotence: stejný požadavek nesmí způsobit duplicitní efekt
- eventual consistency: stav se může mezi službami srovnat až po čase
- circuit breaker: při opakovaných chybách dočasně přerušit volání závislosti

## 2. Example

```python
import time
from urllib.request import urlopen
from urllib.error import URLError


def fetch_with_retry(url: str, attempts: int = 3, timeout: float = 2.0) -> bytes:
    for attempt in range(1, attempts + 1):
        try:
            with urlopen(url, timeout=timeout) as response:
                return response.read()
        except URLError:
            if attempt == attempts:
                raise
            time.sleep(0.2 * attempt)


if __name__ == "__main__":
    try:
        data = fetch_with_retry("https://example.com")
        print("Downloaded bytes:", len(data))
    except URLError:
        print("Služba není dostupná ani po retry")
```

## 3. When to use

* Když navrhujete mikroservisní nebo cloud-native architekturu.
* Když voláte externí API a potřebujete odolnost vůči výpadkům.
* Když řešíte konzistenci dat napříč více službami.

## 4. Common mistakes

* ❌ Retry bez timeoutu a bez limitu pokusů - problém se může zhoršit.
* ❌ Operace bez idempotence při opakování - vznikají duplicitní zápisy.
* ❌ Očekávání silné konzistence všude bez zvážení latency trade-offu.

---

<details>
<summary>Optional: Deep dive</summary>

Distribuované systémy selhávají jinak než lokální aplikace: časté jsou partial failures. Proto je důležitý fallback plán, circuit breaker a dobré SLO metriky (latence, chybovost, dostupnost).

</details>

---

**Další krok:** [Zpět na obsah](../README.md)
