# Lecture Script: SQLAlchemy ORM
**Duration:** 110 minutes | **Tools:** VS Code, venv, SQLite file, Uvicorn | **Language:** FastAPI + SQLAlchemy

**Agenda:** Opening 7 · Why 10 · Concepts 20 · LO walkthroughs 50 · Live demo 13 · Recap 10

---

## Session Opening (7 min)

**[Script:]** "You speak SQL. FastAPI speaks Python. SQLAlchemy is the translator. Today models, CRUD, relationships, and one GET that reads the database — not a list."

**Problem:** `books = []` vs `campus.db`. We want the second behind `/clubs`.

---

## Why Does This Matter?

🎯 **Instructor Note:** Show a SQL typo vs a missing attribute on a model — IDE can help the second.

**[Script:]** "Job descriptions say FastAPI + SQLAlchemy. If you only paste SQL, you fight strings. If you only know the ORM and forget SQL, you cannot debug. We hold both. `commit` is ACID, not a save button with a fancy name."

- **Real-world use:** Almost every Python API with Postgres/SQLite
- **Pain if misunderstood:** Forgot commit (data vanishes); one global session shared across requests (bugs later)

---

## What Is the Concept?

**Model** = class mapped to a table. **Session** = conversation + transaction.

**Mental model:** Class instance = row. `add` = stage. `commit` = stamp.

**Common mistakes:** Mixing Pydantic and ORM models as if identical; not creating tables (`Base.metadata.create_all`); querying after close.

Keep SQLite. No Alembic this session.

---

## How Do We Apply It?

### LO 1: ORM benefits vs raw SQL

**Problem:** Ten routes, ten SQL strings.

**Translate logic:** One `Club` class. Routes use Python.

**Walkthrough:** Benefit list: reuse, relationships, commit/rollback API.

**Predict:** Changing column name in one model is easier than ten queries — still need a migration in real life; mention create_all is class-only.

---

### LO 2: Define SQLAlchemy models

**Write code:**

```python
class Club(Base):
    __tablename__ = "clubs"
    id: Mapped[int] = mapped_column(primary_key=True)
    name: Mapped[str] = mapped_column(String(80), unique=True)
```

**Predict before running:** `create_all` makes `clubs` in SQLite.

**Explain result:** Model is the schema in Python.

---

### LO 3: SELECT INSERT UPDATE DELETE via ORM

**Write code:**

```python
c = Club(name="Robotics")
session.add(c)
session.commit()
c.name = "Robo"
session.commit()
session.delete(c)
session.commit()
```

**Predict before running:** After delete, `session.get(Club, c.id)` is `None`.

**Explain result:** Four verbs, no SQL strings.

---

### LO 4: 1-1, 1-many, many-many in ORM

**Walkthrough (keep short, one field each):**

- 1-many: `Student.club_id` + `Club.students = relationship(...)`
- 1-1: `IdCard.student_id` unique
- many-many: `memberships` table, `secondary=`

**Predict:** Accessing `club.students` runs a SELECT under the hood.

**Explain result:** Relationship types you drew in SQL II become `relationship()`.

---

### LO 5: FastAPI route + simple ORM query

**Write code:**

```python
@app.get("/clubs/{club_id}")
def get_club(club_id: int):
    club = session.get(Club, club_id)
    if club is None:
        raise HTTPException(404, "Not found")
    return {"id": club.id, "name": club.name}
```

**Predict before running:** Empty DB → 404. After insert → JSON.

**Demo:** GET in Swagger.

🎯 **Instructor Note:** "We will inject sessions properly in the e2e session. Today one session is OK if we say it is a teaching shortcut."

---

## Live Demo Block (13 min)

Engine → `create_all` → insert two clubs in a setup block → GET list with `select(Club)` → GET by id → 404.

**[Script:]** "The JSON looks like last month. The source is a file database. That is the whole point."

---

## Recap (10 min)

🎯 **Instructor Note:** "Where is the transaction? Point at commit."

---

## Lecture Summary

- **ORMs** map classes to tables and reduce raw SQL in API code
- **SQLAlchemy models** declare tables and columns
- **ORM CRUD** uses add, get/select, attribute change, delete, plus commit
- **Relationships** express 1-1, 1-many, and many-many
- A **FastAPI route** can return data from `session.get` / `select`
- **Practical value:** Persistent APIs — next we prove who the caller is with JWT
