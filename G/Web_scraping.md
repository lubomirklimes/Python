# Web scraping

Tato kapitola pokrývá získávání dat z webu pomocí BeautifulSoup a Selenium.

## 1. Concept

Web scraping znamená programové čtení obsahu stránek. BeautifulSoup je vhodný pro parsování HTML, které už máte stažené (často přes `requests`). Selenium je nástroj pro automatizaci prohlížeče a hodí se tam, kde je stránka dynamická (JavaScript render).

Python je pro scraping vhodný díky knihovnám a rychlé iteraci. Nevhodný je, pokud porušujete podmínky webu nebo právní limity - vždy respektujte robots.txt, ToS a legislativu.

## 2. Example

```python
from bs4 import BeautifulSoup

html = """
<html><body>
  <ul id='items'>
    <li>Python</li>
    <li>Data</li>
  </ul>
</body></html>
"""

soup = BeautifulSoup(html, "html.parser")
items = [li.get_text(strip=True) for li in soup.select("#items li")]
print(items)

# Selenium se používá na dynamické stránky, např.:
# from selenium import webdriver
# driver = webdriver.Chrome()
# driver.get("https://example.com")
# ...
# driver.quit()
```

## 3. When to use

* Když potřebujete sbírat veřejná data pro monitoring, analýzu nebo interní reporting.
* Když API neexistuje, ale data jsou legálně dostupná přes webové rozhraní.

## 4. Common mistakes

* ❌ Ignorování právních a etických pravidel scrapingu.
* ❌ Agresivní požadavky bez rate limiting - hrozí blokace nebo přetížení serveru.

---

<details>
<summary>Optional: Deep dive</summary>

Preferujte oficiální API, pokud existuje. Scraping je křehký vůči změnám HTML struktury, proto je dobré mít monitoring kvality dat a fallback strategii.

</details>

---

**Další krok:** [DevOps a scripting](DevOps_a_scripting.md)
