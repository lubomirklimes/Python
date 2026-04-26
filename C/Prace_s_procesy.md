# Práce s procesy

V této kapitole si ukážeme, jak spouštět externí programy bezpečně pomocí modulu `subprocess`.

## 1. Concept

`subprocess` umožňuje spustit jiný proces, předat mu argumenty a získat výstup. V moderním Pythonu je nejběžnější funkce `subprocess.run()`.

Bezpečný výchozí přístup:

- předávat příkaz jako seznam argumentů, ne jako jeden string
- používat `shell=False` (výchozí), pokud to není nutné
- používat `check=True`, aby se chyba procesu promítla do výjimky
- nastavovat timeout u potenciálně dlouhých příkazů

## 2. Example

```python
import subprocess

result = subprocess.run(
    ["python", "--version"],
    capture_output=True,
    text=True,
    check=True,
    timeout=5,
)

print("STDOUT:", result.stdout.strip())
print("STDERR:", result.stderr.strip())
print("Return code:", result.returncode)
```

## 3. When to use

* Když potřebujete z Pythonu spustit externí nástroj (git, ffmpeg, systémové utilitky).
* Když stavíte automatizační skripty a chcete zachytit výstup volaného procesu.

## 4. Common mistakes

* ❌ `shell=True` s neověřeným uživatelským vstupem – riziko command injection.
* ❌ Ignorování návratového kódu – skript vypadá úspěšně, ale podřízený proces selhal.

---

<details>
<summary>Optional: Deep dive</summary>

Pro dlouho běžící procesy nebo streamovaný výstup je vhodné použít `subprocess.Popen` a číst data průběžně. U paralelních úloh zvažte frontu a omezení počtu současně běžících procesů.

</details>

---

**Další krok:** [Logging](Logging.md)
