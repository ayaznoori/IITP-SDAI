# Lecture Script: File Handling & CORS
**Duration:** 110 minutes | **Tools:** VS Code, Uvicorn, browser, optional Vite origin note | **Language:** FastAPI

**Agenda:** Opening 7 · Why 12 · Concepts 18 · LO walkthroughs 50 · Live demo 13 · Recap 10

---

## Session Opening (7 min)

**[Script:]** "Your React app and FastAPI are two restaurants on two streets. The browser is the police. **CORS** is the permit. Then we upload a file, download a file, and log **after** we reply — background tasks."

**Problem:** Fetch from last module's React todo to today's API. Console: blocked by CORS. Postman was fine. Students think FastAPI is down.

---

## Why Does This Matter?

🎯 **Instructor Note:** Reproduce CORS error if a simple HTML file on another port fetches `/health`. Show the console.

**[Script:]** "**Google Drive**, **Notion** image paste, **Naukri** resume upload — files plus APIs. If you block CORS badly, frontends die. If you do heavy logging before responding, the UI feels slow. Background tasks fix the second. CORS middleware fixes the first."

- **Real-world use:** Assignment portals, KYC document upload, export CSV download
- **Pain if misunderstood:** `allow_origins=["*"]` copied forever; huge `file.read()` without thinking; background task for work the user must wait for

---

## What Is the Concept?

**Origin** = scheme + host + port. `localhost:5173` ≠ `127.0.0.1:8000`.

**CORSMiddleware** adds allow headers so browsers permit JS to read the response.

**UploadFile** = incoming multipart file. **FileResponse** = outgoing file.

**BackgroundTasks** = functions queued to run after the response.

**Mental model:** CORS = visitor pass. Upload = inbound parcel. Download = outbound parcel. Background = mop the floor after the customer left.

**Common mistakes:** Testing CORS only in Postman; returning JSON with a file path instead of bytes; `add_task` for the only copy of the data.

---

## How Do We Apply It?

### LO 1: Configure CORS middleware

**Write code:**

```python
from fastapi.middleware.cors import CORSMiddleware

app.add_middleware(
    CORSMiddleware,
    allow_origins=["http://localhost:5173"],
    allow_credentials=False,
    allow_methods=["*"],
    allow_headers=["*"],
)
```

**Predict before running:** Browser fetch from that origin succeeds. A random origin still blocked.

**Explain result:** Same middleware pattern as timing headers, specialized for CORS.

---

### LO 2: Add background tasks

**Write code:**

```python
from fastapi import BackgroundTasks, FastAPI

app = FastAPI()

def ping_log():
    open("data/hits.txt", "a").write("hit\n")

@app.post("/ping")
def ping(background_tasks: BackgroundTasks):
    background_tasks.add_task(ping_log)
    return {"ok": True}
```

**Predict before running:** Response is instant. `hits.txt` grows after.

**Explain result:** User does not wait on the disk write (tiny here, pattern scales).

---

### LO 3: Basic file upload

**Write code:**

```python
from fastapi import File, UploadFile

@app.post("/upload")
async def upload(file: UploadFile = File(...)):
    data = await file.read()
    return {"filename": file.filename, "bytes": len(data)}
```

**Predict before running:** Swagger shows file picker. JSON reports size.

**Explain result:** Not a JSON body — **form** upload. `/docs` knows.

🎯 **Instructor Note:** Upload a small `.txt` only in class.

---

### LO 4: File download response

**Write code:**

```python
from fastapi.responses import FileResponse

@app.get("/files/notes")
def notes():
    return FileResponse("data/notes.txt", media_type="text/plain", filename="notes.txt")
```

**Predict before running:** Browser downloads or displays text, not `{"path": ...}`.

**Explain result:** `FileResponse` sets headers for a file payload.

---

### LO 5: When background vs regular handling

**Problem:** Must confirm upload saved; also log.

**Translate logic:** `await file.read()` + write to disk **in the handler**. `add_task` for append-only log.

**Walkthrough table:**

| Job | Where |
|-----|--------|
| Validate file exists | Request |
| Write bytes to `data/uploads/` | Request (client needs truth) |
| Append "uploaded resume.pdf" | Background |
| CORS headers | Middleware |

**Predict:** If log is in background and process dies instantly, log might miss — mention briefly, no Celery.

---

## Live Demo Block (13 min)

1. CORS on, show React-or-fetch vs Postman.  
2. Upload txt, print size.  
3. Download `notes.txt`.  
4. Ping endpoint; tail `hits.txt`.

**[Script:]** "If the user must know it worked, do it in the request. If it is a diary note, background is fine."

---

## Recap (10 min)

🎯 **Instructor Note:** "Why Postman ignored CORS?" (not a browser).

---

## Lecture Summary

- **CORS middleware** lets a browser frontend on another origin call the API
- **Background tasks** run after the response is sent
- **UploadFile** implements basic file upload
- **FileResponse** returns a downloadable file
- **Choose background vs request** based on whether the client must wait for the work
- **Practical value:** Your API can talk to React and move files — next we persist structured data in SQL
