# WebSockets

Tato kapitola vysvětluje realtime komunikaci přes WebSockets oproti klasickému request/response modelu.

## 1. Concept

WebSocket drží otevřené obousměrné spojení mezi klientem a serverem. To je vhodné pro chat, live notifikace, dashboardy nebo streamování událostí. Na rozdíl od REST API nemusíte pro každou změnu opakovaně posílat nový HTTP request.

Při návrhu myslete na správu připojení, heartbeat, autorizaci a limity počtu klientů.

## 2. Example

```python
from fastapi import FastAPI, WebSocket, WebSocketDisconnect

app = FastAPI()


@app.websocket("/ws")
async def websocket_endpoint(websocket: WebSocket) -> None:
    await websocket.accept()
    try:
        while True:
            text = await websocket.receive_text()
            await websocket.send_text(f"Echo: {text}")
    except WebSocketDisconnect:
        print("Klient se odpojil")
```

## 3. When to use

* Když potřebujete nízkou latenci a průběžné aktualizace v reálném čase.
* Když polling přes REST vytváří zbytečnou zátěž a horší uživatelský zážitek.

## 4. Common mistakes

* ❌ Chybějící autorizace WS spojení – endpoint může být otevřený komukoliv.
* ❌ Neřešené odpojování a cleanup připojení – postupně dochází prostředky serveru.

---

<details>
<summary>Optional: Deep dive</summary>

Realtime architektura často potřebuje message broker (např. Redis) pro škálování na více instancí. Bez sdíleného stavu se zprávy mezi instancemi těžko synchronizují.

</details>

---

**Další krok:** [Zpět na obsah](../README.md)
