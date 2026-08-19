# Lecture Script: Backend Setup with FastAPI
**Duration:** 110 minutes | **Tools:** VS Code, Terminal, venv, browser | **Language:** Python + FastAPI

**Agenda:** Opening 8 · Why 10 · Concepts 18 · LO walkthroughs 50 · Live demo 14 · Recap 10

---

## Session Opening (8 min)

**[Script:]** "For weeks you called APIs. JSONPlaceholder was a fake kitchen. Today we open **our** kitchen. FastAPI turns Python functions into HTTP endpoints. By the break you will have a server on port 8000 answering GET."

**Problem hook:** A campus club React page stores members only in `localStorage`. Another student on another laptop never sees them. We need a shared backend.

🎯 **Instructor Note:** Draw three boxes: Browser | FastAPI | (database later). Circle FastAPI. "We only build the middle box today."

---

## Why Does This Matter?

🎯 **Instructor Note:** Ask: "Who owns the data when it lives only in the browser?" Wait for "the user's machine."

**[Script:]** "Every product you use — **IRCTC**, **WhatsApp Web**, **Zomato** — has a backend. If you skip venv, classmates' packages collide. If you never run Uvicorn, you cannot prove an endpoint works. If GET is unclear, you will confuse it with POST later and break browsers."

- **Real-world use:** Health checks, public config, "is the API up?" probes in every cloud deploy
- **Pain if misunderstood:** Installing FastAPI globally, then another course's package versions break this project

---

## What Is the Concept?

### Backend role

**Definition:** The backend is the server-side program that handles HTTP, applies rules, and returns data.

**Mental model:** Request in, JSON out. The function is a waiter ticket.

| Layer | You already did | Today |
|-------|-----------------|-------|
| Frontend | HTML, CSS, React, Fetch | Still the client |
| Backend | Consumed JSONPlaceholder | **Write** FastAPI |
| Database | — | Later sessions |

**Python vs JS:** `fetch('/health')` in the browser is the client. `@app.get("/health")` is the server. Same HTTP, two languages.

**Common mistake:** Thinking FastAPI replaces React. It does not.

### venv for a backend project

Same as Module 1: isolate packages. Always activate before `pip install`.

**Common mistake:** Running Uvicorn with system Python so FastAPI is "not found."

### FastAPI + Uvicorn

**FastAPI** = framework (routes, JSON). **Uvicorn** = server process listening on a port.

`uvicorn main:app --reload` means: load `app` from `main.py`, watch files.

### Minimal app + GET

A **GET** asks for a resource. No body required. Browser address bar always sends GET.

```python
from fastapi import FastAPI
app = FastAPI()

@app.get("/ping")
def ping():
    return {"ok": True}
```

**Common mistake:** Naming the file `fastapi.py` — import shadowing.

---

## How Do We Apply It?

### LO 1: Explain the role of backend development

**Problem:** User list must be shared across phones.

**Translate logic:** UI cannot be the source of truth. A server holds the list.

**Walkthrough:** Sketch "GET /members" returning `[{"name": "Asha"}]`. Two browsers, same JSON.

**Predict before running:** N/A — whiteboard. Ask: "Does React still exist?" Yes.

**Explain result:** Backend = shared data + rules. Frontend = display.

---

### LO 2: Create and activate a virtual environment

**Problem:** Two projects need different package versions.

**Translate logic:** One folder, one venv, one `pip`.

**Write code (terminal):**

```bash
python3 -m venv venv
source venv/bin/activate
which python
```

**Predict before running:** Path includes `.../venv/bin/python`.

**Explain result:** Isolation is working. Prompt shows `(venv)`.

🎯 **Instructor Note:** Have one volunteer skip activate and fail on purpose. Class diagnoses it.

---

### LO 3: Install FastAPI and run the development server

**Problem:** Framework is not on disk yet.

**Write code:**

```bash
pip install "fastapi[standard]"
uvicorn main:app --reload
```

(Create empty `main.py` with `app` first, or install then write.)

**Predict before running:** Terminal prints `Uvicorn running on http://127.0.0.1:8000`.

**Explain result:** Process is listening. Ctrl+C stops it.

**Demo 1 (≤10 lines) — wait until `main.py` exists in LO 4.**

---

### LO 4: Build a minimal FastAPI application

**Problem:** Need an `app` object Uvicorn can import.

**Write code:**

```python
from fastapi import FastAPI

app = FastAPI(title="Campus API")

@app.get("/")
def root():
    return {"service": "campus-api"}
```

**Predict before running:** Browser at `/` shows JSON with `service`.

**Explain result:** Decorator registered the path. Return dict → JSON automatically.

🎯 **Instructor Note:** Pause if import error — wrong venv or file name.

---

### LO 5: Implement and test a basic GET

**Problem:** Ops wants a health check.

**Write code:**

```python
@app.get("/health")
def health():
    return {"status": "ok"}
```

**Predict before running:** `/health` → `{"status":"ok"}`, status 200.

**Demo 2:** Open `/`, then `/health`. Show Network tab: method GET.

**Explain result:** Two routes, one app. Typos in the path 404 — FastAPI JSON error, not HTML.

---

## Live Demo Block (14 min)

Build `campus-api/main.py` from zero: venv → install → two GET routes → `--reload` → edit message → browser refreshes after save.

**[Script:]** "Watch `--reload`. I change the string. I save. I refresh. No manual restart. That is why we use Uvicorn in class."

---

## Recap (10 min)

🎯 **Instructor Note:** Cold call: "What does `main:app` mean?" "Why not install FastAPI globally?"

---

## Lecture Summary

- **Backend** serves shared data and rules over HTTP; frontend remains the client
- **venv** isolates this project's Python and packages
- **FastAPI + Uvicorn** install and run a local development server
- A **minimal app** is `FastAPI()` plus decorated path functions
- A **basic GET** is tested in the browser and returns JSON with 200
- **Practical value:** You own a real API process — the foundation for CRUD, validation, and databases next

**[Script:]** "Next session we add POST, PUT, DELETE — the verbs a todo API actually needs. Bring this project folder."
