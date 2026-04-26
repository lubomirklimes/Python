# Kubernetes

Tato kapitola představuje základní stavební bloky Kubernetes a jejich roli při provozu Python aplikací.

## 1. Concept

Kubernetes řeší orchestraci kontejnerů: plánování běhu, restart při pádu, škálování a service discovery. Pro běžné nasazení API obvykle používáte:

- pod: běžící jednotka (1+ kontejnerů)
- deployment: deklarace požadovaného stavu aplikace (počet replik, image)
- service: stabilní síťový endpoint k deploymentu

Kubernetes je silný pro větší týmy a více služeb. Pro malý interní projekt může být zbytečně složitý oproti jednoduchému kontejner hostingu.

## 2. Example

```python
from pathlib import Path

manifest = """
apiVersion: apps/v1
kind: Deployment
metadata:
  name: fastapi-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: fastapi-app
  template:
    metadata:
      labels:
        app: fastapi-app
    spec:
      containers:
      - name: app
        image: my-fastapi-app:latest
        ports:
        - containerPort: 8000
---
apiVersion: v1
kind: Service
metadata:
  name: fastapi-service
spec:
  selector:
    app: fastapi-app
  ports:
  - port: 80
    targetPort: 8000
  type: ClusterIP
""".strip()

Path("k8s.yaml").write_text(manifest + "\n", encoding="utf-8")
print("Manifest uložen do k8s.yaml")
```

## 3. When to use

* Když provozujete více mikroservis a potřebujete standardizovaný deployment.
* Když chcete automatické škálování a self-healing chování.
* Když řešíte multi-team provoz v cloudu s vysokou dostupností.

## 4. Common mistakes

* ❌ Nasazení Kubernetes bez observability a alertingu - provoz je pak nepředvídatelný.
* ❌ Převzetí výchozích limitů zdrojů bez měření - hrozí throttling nebo OOM.
* ❌ Použití Kubernetes pro velmi malý projekt bez provozní kapacity týmu.

---

<details>
<summary>Optional: Deep dive</summary>

Kubernetes řeší hlavně orchestraci, ne business logiku aplikace. Bez kvalitních health checků, readiness probe a rollback strategie nezískáte plný benefit platformy.

</details>

---

**Další krok:** [Cloud aplikace v Pythonu](Cloud_aplikace.md)
