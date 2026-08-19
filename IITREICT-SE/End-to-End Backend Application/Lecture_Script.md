# Lecture Script: End-to-End Backend Application
**Duration:** 110 minutes | **Tools:** VS Code, Uvicorn, SQLite, Swagger | **Format:** Instructor-led mini app (campus clubs)

**Agenda:** Opening 8 · Why 10 · Architecture 15 · LO walkthroughs + live build 62 · Recap 15

This session **assembles** prior LOs. Do not introduce new frameworks.

---

## Session Opening (8 min)

**[Script:]** "No new universe today. We clip FastAPI, Pydantic, SQLAlchemy, JWT, and Depends into one **campus clubs** service. If you can trace one POST from header to SQLite, you can start a capstone backend."

**Problem:** Pieces work in isolation. Students cannot draw the flow. We build it live.

🎯 **Instructor Note:** Empty folder `campus_e2e/`. Students type along. Pause every file.

---

## Why Does This Matter?

🎯 **Instructor Note:** Restart demo at the end. Data remains. Contrast week-1 memory list.

**[Script:]** "Interviews ask 'explain what happens when I hit your API.' **Razorpay**, **Freshworks**, **your internship** — this diagram. If sessions leak between requests, users see each other's writes. If you skip auth on POST, the database fills with junk. This is the dress rehearsal for Module 7."

- **Real-world use:** Any CRUD admin + public read
- **Pain if misunderstood:** Forgot `Depends(get_db)`; commit never called; protecting GET by accident and breaking the demo UI later

---

## What Is the Concept?

**End-to-end backend flow:** HTTP → (auth) → validate → ORM session → SQL → commit → response model.

**Session wiring:** `get_db` generator dependency.

**Selected protection:** public reads, admin writes, public login.

**Mental model:** Assembly line. Each station is a prior lecture.

**Common mistakes:** Mixing Pydantic model into `session.add`; returning ORM objects with lazy relationships exploding; committing in GET.

---

## How Do We Apply It?

Build in order. Each LO is a checkpoint. Demos stay short; the long demo is the app.

### LO 1: Explain the full backend flow from API to DB

**Whiteboard (5 min):** Numbered arrows. Students copy.

**Predict:** Where does 422 happen vs 401 vs 404 vs 500?

**Explain result:** Status codes locate the station.

---

### LO 2: CRUD with persistent ORM storage

**Problem:** Clubs must survive reload.

**Write/live:** `Club` model, `create_all`, POST insert + commit, GET select, PUT, DELETE.

**Predict before running:** Kill Uvicorn, start again, GET /clubs still lists committed rows.

**Explain result:** SQLite file is the source of truth.

---

### LO 3: Wire SQLAlchemy sessions with FastAPI

**Write code:**

```python
def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()
```

Every route that touches DB takes `db: Session = Depends(get_db)`.

**Predict before running:** Two parallel Swagger tabs do not share one dirty session object.

**Explain result:** Per-request session ≈ per-request transaction boundary. Commit on success in the route.

🎯 **Instructor Note:** Show `try: ... commit except: rollback raise`.

---

### LO 4: Protect selected endpoints with prior auth

**Live:** `POST /login` from JWT session. `get_current_user` + `require_admin` on POST/PUT/DELETE only.

**Predict before running:** GET /clubs anonymous 200. POST without token 401. Student token 403. Admin 201.

**Explain result:** Same security session, now on real CRUD.

---

### LO 5: Follow instructor-led demo of a mini backend app

**Script the demo path (do not skip):**

1. File tree: `main.py`, `models.py`, `schemas.py`, `db.py`, `auth.py`  
2. Seed admin hash once  
3. Login in `/docs` → Authorize  
4. Create club → list → update → delete → 404 get  
5. Restart → list still there  
6. Point at flow diagram after each status code

**[Script:]** "If you get lost, ask: which file is this station?"

Keep feature set tiny. No file upload, no pagination extras unless leftover time.

---

## Live Demo Block

The LO 2–5 build **is** the 62-minute demo. Guard time: login working by minute 70; persistence proof by minute 95.

---

## Recap (15 min)

🎯 **Instructor Note:** Cold call five students: name one file and its job. Draw the pipe from memory.

**[Script:]** "Module 3 evaluations are yours to take without us writing them. You now have a backend you can explain. Next module: LLMs — and you will hang AI on this same FastAPI skill."

---

## Lecture Summary

- The **backend flow** is route → validate → session → SQL → JSON
- **CRUD** is backed by a real ORM database, not a list
- **`get_db`** wires a session per request and closes it
- **JWT dependencies** protect write routes; reads and login stay public as designed
- The **mini app** is the assembly of every Module 3 lecture
- **Practical value:** You can build and narrate a small production-shaped API — the capstone backend starts here, not from zero
