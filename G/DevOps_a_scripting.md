# DevOps a scripting

Tato kapitola ukazuje, jak Python využít v CI/CD a provozní automatizaci.

## 1. Concept

V DevOps se Python často používá pro skripty kolem buildu, testů, release procesů, infrastruktury a reportingu. Může orchestrace spojovat různé nástroje (git, cloud CLI, test runner) do jednoho opakovatelného workflow.

Python je vhodný pro glue code a automatizaci. Méně vhodný je tam, kde už existuje stabilní nativní nástroj s lepší integrací a menší údržbou.

## 2. Example

```python
import subprocess
from pathlib import Path


def run_step(command: list[str]) -> None:
    print("RUN:", " ".join(command))
    subprocess.run(command, check=True)


if Path("pyproject.toml").exists():
    run_step(["python", "-m", "pytest", "-q"])
else:
    print("Projekt nema pyproject.toml, test krok preskocen")
```

## 3. When to use

* Když chcete standardizovat build/test/deploy kroky mezi lokálním a CI prostředím.
* Když potřebujete automatizovat provozní úlohy (zálohy, kontrola služeb, cleanup).

## 4. Common mistakes

* ❌ Skripty závislé na lokálních předpokladech bez dokumentace (cesty, env proměnné).
* ❌ Chybějící návratové kódy a stopnutí pipeline při selhání.

---

<details>
<summary>Optional: Deep dive</summary>

Pro CI je důležité, aby skript byl deterministický: stejné vstupy, stejné výstupy. Vyplatí se explicitní timeouty, retry strategie pro nestabilní kroky a strukturované logování.

</details>

---

**Další krok:** [IoT a hardware](IoT_a_hardware.md)
