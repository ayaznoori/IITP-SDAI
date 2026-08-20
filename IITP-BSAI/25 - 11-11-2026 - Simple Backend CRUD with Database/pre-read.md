# Pre-Read: Simple Backend CRUD with Database

## Session Mindmap

This diagram shows where this session sits in the course.

```mermaid
flowchart TB
    subgraph foundation ["Foundation"]
        direction TB
        P1["<b>Previous Module</b><br/>Module 6: Backend FastAPI<br/><i>[CRUD · CORS]</i><br/>HTTP verbs in memory"]
        P2["<b>Previous Module</b><br/>Module 7 until ORM<br/><i>[Neon · SQLAlchemy]</i><br/>One model and GET read"]
        CM["<b>Current Module Until Previous Session</b><br/>Module 7: Database<br/><i>[SQL · ORM]</i><br/>Schema plus one read"]
    end

    subgraph current ["Current Session"]
        CS["<b>Current Session</b><br/>Simple Backend CRUD with Database<br/><i>Mental shift:</i> from <b>RAM lists</b> to <b>committed rows</b><br/>Session · CREATE/READ · one mutation"]
    end

    subgraph value ["Course & Real-Life Value"]
        CV["<b>Course Value</b><br/>API ready for tests and React"]
        RL["<b>Real-Life Use</b><br/>Tickets, notes, and carts that survive deploy"]
    end

    subgraph future ["Upcoming Modules"]
        direction TB
        U1["<b>Upcoming Module</b><br/>Module 8: Testing Hygiene<br/><i>[Pytest · CRUD lab]</i><br/>Checks and full-stack lab"]
        U2["<b>Upcoming Module</b><br/>Module 9: Deploy Ops<br/><i>[Docker · CI]</i><br/>Run the API anywhere"]
        U3["<b>Upcoming Module</b><br/>Module 10: Software 3.0<br/><i>[LLM APIs]</i><br/>AI on a real backend"]
    end

    P1 ==>|&nbsp;Foundation&nbsp;| P2
    P2 ==>|&nbsp;Builds on&nbsp;| CM
    CM ==>|&nbsp;Blueprint&nbsp;| CS
    CS ==>|&nbsp;Course Path&nbsp;| CV
    CS ==>|&nbsp;Real-Life Use&nbsp;| RL
    CS ==>|&nbsp;Next Module&nbsp;| U1
    U1 -.-> U2
    U2 -.-> U3

    classDef previous fill:#E8F4FD,stroke:#4A90D9,stroke-width:2px,color:#1a1a1a
    classDef current fill:#FFF3CD,stroke:#E6A817,stroke-width:3px,color:#1a1a1a
    classDef value fill:#D4EDDA,stroke:#28A745,stroke-width:2px,color:#1a1a1a
    classDef future fill:#F3E8FF,stroke:#9B59B6,stroke-width:2px,color:#1a1a1a

    class P1,P2,CM previous
    class CS current
    class CV,RL value
    class U1,U2,U3 future

    linkStyle default stroke-width:3px
```

## 1. What You'll Learn

In this pre-read, you'll discover:

- How FastAPI **routes share an ORM session** with the database
- How **CREATE and READ** persist rows, not just RAM
- How to add **one UPDATE or one DELETE** against Postgres
- How to **prove data survives** a server restart
- How to **test the flow in Swagger** before touching React

---

## 2. Detailed Explanation

### The Problem You Already Felt

In-memory lists reset when Uvicorn restarts. Users do not accept disappearing tasks.

**Persistent CRUD** means create, read, update, or delete **rows**. The database keeps them.

**Analogy:** A whiteboard is RAM. A notebook is Neon. CRUD writes in the notebook.

> **In the Real World:** **Todoist** and **Trello** would be toys if cards vanished on deploy. Every product you use stores mutations in a database.

**Why It Matters**

- Restarts and deploys keep user data
- Swagger tests the contract before the UI
- One resource done well beats five half-wired tables

**Benefits**

- Real backend portfolio proof
- Clear 201 / 404 habits
- Same session pattern for every future table

### Wire Routes to Sessions

Each request gets a **session** (a short DB conversation), then closes it.

```python
def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()
```

Routes declare `db: Session = Depends(get_db)`. FastAPI injects it.

### Persistent CREATE and READ

**CREATE:** take a Pydantic body → make a model instance → `db.add` → `db.commit` → return the row.

**READ:** `db.get` or `db.scalars(select(Note)).all()` → JSON list or one item.

```python
@app.post("/notes")
def create_note(payload: NoteIn, db: Session = Depends(get_db)):
    note = Note(title=payload.title)
    db.add(note)
    db.commit()
    db.refresh(note)
    return note
```

Keep this under ten lines in your notes. Details live in lecture.

### One UPDATE or DELETE

You do **one** mutation besides create/read. Pick UPDATE **or** DELETE. Do it well.

**UPDATE idea:** load row, change field, commit.

**DELETE idea:** load row, `db.delete`, commit.

Always handle “row missing” with a 404.

> **In the Real World:** **GitHub Issues** PATCH title or DELETE an issue. Same two verbs. You ship one of them today.

### Verify After Restart

1. POST a note in Swagger.
2. Stop Uvicorn.
3. Start Uvicorn.
4. GET the same id.

If the note remains, you left RAM behind.

### Test in Swagger

Open `/docs`. Try POST, GET, then PATCH or DELETE. Read status codes. Do not skip error cases.

**Messy to Clear**

**Messy:** Mix list storage and database in the same route.

**Clear:** Every CREATE/READ/UPDATE/DELETE uses `db` only.

### Building Blocks Checklist

- [ ] I can explain `Depends(get_db)`
- [ ] I know `add` / `commit` / `refresh`
- [ ] I can describe one UPDATE or DELETE
- [ ] I have a restart test plan
- [ ] I can click the flow in Swagger

---

## 3. Practice Exercises

**Exercise 1 — Session**
In two sentences, why we close the session in `finally`.

**Exercise 2 — CREATE**
Order these: `commit`, `Note(...)`, `add`, `refresh`.

**Exercise 3 — READ**
Write the GET path for one note by id. Example: `/notes/3`.

**Exercise 4 — Mutation**
Choose UPDATE or DELETE. Write the happy path in four bullets.

**Exercise 5 — Restart**
Write the four-step restart test. Circle the step that proves persistence.
