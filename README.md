# TCP/IP Protocol Flow Visualizer

A small FastAPI app with a minimal front end that shows how a request travels the network
(DNS, TCP, then HTTP or SMTP) as a live 3D scene plus a step-by-step sequence diagram.

- **Backend:** `main.py` (FastAPI) serves the page and three endpoints:
  `POST /api/browse`, `POST /api/mail`, `POST /api/stream`.
  Each returns `{"sequence": [{type, sender, receiver, protocol, msg}, ...]}`.
- **Frontend:** `index.html` (one file, no build step). Three.js and fonts load from a CDN.
  If the API can't be reached, the page shows an "Offline demo" copy of the same data.

## Version history

This is the **updated version of `app_layer_workflow01`**.

| Version | Layers covered |
| --- | --- |
| `app_layer_workflow01` (previous) | Application layer view only |
| **This version** | Application layer **plus the Transport layer** (TCP three-way handshake) |

### Roadmap

The goal is to cover every layer of the TCP/IP model in future updates:

| Layer | Status |
| --- | --- |
| Application (DNS, HTTP, SMTP, HLS streaming) | Done |
| Transport (TCP) | Done in this version |
| Internet (IP) | Planned |
| Link / Network access | Planned |

## Notes

Only the DNS lookup in `/api/browse` is real (`socket.gethostbyname` on the server).
The TCP, HTTP and SMTP steps are illustrative text.

## Run locally

```bash
pip install -r requirements.txt
uvicorn main:app --reload
```

Open http://localhost:8000. The top bar shows **Live backend** when the API responds.

## Deploy on Render

1. Push this repo to GitHub.
2. In Render: New > Web Service > pick the repo.
3. Build command: `pip install -r requirements.txt`
4. Start command: `uvicorn main:app --host 0.0.0.0 --port $PORT`

(`render.yaml` already contains these settings, so a Blueprint deploy also works.)
Free instances sleep when idle, so the first request can take up to a minute.

## Files

| File | Purpose |
| --- | --- |
| `main.py` | FastAPI backend |
| `index.html` | UI (3D stage, sequence diagram, playback controls) |
| `requirements.txt` | Python dependencies |
| `render.yaml` | Optional Render config |
