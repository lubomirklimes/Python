# Docker

Tato kapitola vysvětluje, co je Docker a proč je důležitý pro vývoj i nasazení Python aplikací.

## 1. Concept

Docker balí aplikaci a její závislosti do image, kterou spustíte jako kontejner. Díky tomu běží aplikace konzistentně na notebooku vývojáře, v CI i v produkci. U Pythonu tím řešíte hlavně rozdílné verze interpreteru, knihoven a systémových balíčků.

Základní pojmy:

- image: šablona aplikace (souborový systém + metadata)
- container: běžící instance image
- volume: trvalé úložiště mimo životní cyklus kontejneru
- port mapping: propojení portu v kontejneru s hostitelem

Pro lokální vývoj bývá image jednodušší (debug nástroje, hot reload). Produkční image má být menší, bezpečnější a reprodukovatelná.

## 2. Example

```python
from pathlib import Path

# Dockerfile pro jednoduchou FastAPI aplikaci.
dockerfile = """
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt ./
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
EXPOSE 8000
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
""".strip()

Path("Dockerfile").write_text(dockerfile + "\n", encoding="utf-8")

print("Dockerfile byl vytvořen.")
print("Spuštění image/containeru:")
print("docker build -t my-fastapi-app .")
print("docker run -p 8000:8000 my-fastapi-app")
```

## 3. When to use

* Když chcete mít stejné prostředí mezi lokálním vývojem, CI a produkcí.
* Když nasazujete API nebo worker procesy na cloud infrastrukturu.
* Když potřebujete izolovat aplikaci od hostitelského systému.

## 4. Common mistakes

* ❌ Image obsahuje development nástroje i tajné klíče - zvyšuje to riziko i velikost.
* ❌ Chybí fixace verzí závislostí - build není reprodukovatelný.
* ❌ Aplikace zapisuje kritická data do kontejneru bez volume - data se při restartu ztratí.

---

<details>
<summary>Optional: Deep dive</summary>

U produkčních image je vhodné používat multi-stage build, non-root uživatele a pravidelný security scanning image. Prakticky tím snížíte attack surface i riziko driftu prostředí.

</details>

---

**Další krok:** [Kubernetes](Kubernetes.md)
