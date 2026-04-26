# Flask

Tato kapitola ukazuje, jak vytvořit jednoduchou webovou aplikaci ve Flasku.

## 1. Concept

Flask je lehký framework, který vám dá routing a základní HTTP vrstvy bez silných konvencí. Je vhodný pro menší služby, interní nástroje nebo MVP, kde chcete mít kontrolu nad strukturou.

Flask dává minimum "magie", takže si sami rozhodnete, jak budete organizovat validaci, autentizaci i datovou vrstvu.

## 2. Example

```python
from flask import Flask, request, jsonify

app = Flask(__name__)


@app.get("/health")
def health() -> tuple[dict[str, str], int]:
    return {"status": "ok"}, 200


@app.post("/echo")
def echo() -> tuple[dict[str, str], int]:
    data = request.get_json(silent=True) or {}
    name = str(data.get("name", "světe")).strip()
    if not name:
        return {"error": "Pole 'name' je povinné."}, 400
    return jsonify({"message": f"Ahoj, {name}!"}), 200


if __name__ == "__main__":
    app.run(debug=True)
```

## 3. When to use

* Když potřebujete rychle postavit menší web/API službu.
* Když chcete framework s nízkou složitostí a postupně přidávat jen to, co potřebujete.

## 4. Common mistakes

* ❌ Spoléhat na `debug=True` v produkci – bezpečnostní i provozní riziko.
* ❌ Chybějící validace JSON vstupu – endpoint vrací nejasné chyby.

---

<details>
<summary>Optional: Deep dive</summary>

U Flasku se vyplatí časem zavést application factory pattern, oddělit blueprints a přidat jednotnou vrstvu pro error handling. Tím si připravíte cestu k většímu projektu bez velkého refaktoringu.

</details>

---

**Další krok:** [Django](Django.md)
