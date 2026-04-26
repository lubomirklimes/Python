# Networking

Tato kapitola pokrývá základní HTTP komunikaci přes `requests` a nízkoúrovňový modul `http.client`.

## 1. Concept

Pro většinu aplikací je praktická výchozí volba knihovna `requests`, protože nabízí jednoduché API, přehledné zpracování odpovědi a snadné nastavení timeoutu. Modul `http.client` je součást standardní knihovny a hodí se, když nechcete externí závislost nebo potřebujete nižší úroveň.

Bezpečný základ pro HTTP volání:

- vždy nastavte timeout
- kontrolujte stavový kód (`raise_for_status()` nebo vlastní kontrola)
- validujte odpověď, nepracujte slepě s daty

## 2. Example

```python
import json
import http.client

# requests varianta (externí knihovna)
try:
    import requests

    response = requests.get("https://httpbin.org/get", timeout=5)
    response.raise_for_status()
    print(response.json()["url"])
except ImportError:
    print("Knihovna 'requests' neni nainstalovana.")

# http.client varianta (standardní knihovna)
conn = http.client.HTTPSConnection("httpbin.org", timeout=5)
conn.request("GET", "/get")
res = conn.getresponse()
body = res.read().decode("utf-8")
conn.close()

if res.status != 200:
    raise RuntimeError(f"HTTP chyba: {res.status}")

parsed = json.loads(body)
print(parsed["url"])
```

## 3. When to use

* Když voláte REST API z backendu, integračního skriptu nebo CLI nástroje.
* Když potřebujete rychle napsat spolehlivé HTTP volání s timeoutem a kontrolou chyb.

## 4. Common mistakes

* ❌ Volání bez timeoutu – při výpadku sítě může proces dlouho viset.
* ❌ Ignorování HTTP chyb (4xx/5xx) – aplikace pak pracuje s neplatnými daty.

---

<details>
<summary>Optional: Deep dive</summary>

Pro vyšší zátěž se vyplatí reuse připojení (`requests.Session`) a strategie opakování (`retry`) pro dočasné chyby sítě. U citlivých API dává smysl logovat request ID a latenci pro troubleshooting.

</details>

---

**Další krok:** [Práce s procesy](Prace_s_procesy.md)
