# Pre-Read: ORM, Neon Connection & Schema Design

## Session Mindmap

This diagram shows where this session sits in the course.

```mermaid
flowchart TB
    subgraph foundation ["Foundation"]
        direction TB
        P1["<b>Previous Module</b><br/>Module 6: Backend FastAPI<br/><i>[Uvicorn · Pydantic]</i><br/>In-memory JSON APIs"]
        P2["<b>Previous Module</b><br/>Module 5: React Frontend<br/><i>[Fetch · Router]</i><br/>UI that calls APIs"]
        CM["<b>Current Module Until Previous Session</b><br/>Module 7: Database<br/><i>[Neon · SQL]</i><br/>Tables, keys, and joins"]
    end

    subgraph current ["Current Session"]
        CS["<b>Current Session</b><br/>ORM, Neon Connection & Schema Design<br/><i>Mental shift:</i> from <b>SQL strings</b> to <b>Python models</b><br/>SQLAlchemy · URL · one GET read"]
    end

    subgraph value ["Course & Real-Life Value"]
        CV["<b>Course Value</b><br/>Persistent backend for full-stack CRUD"]
        RL["<b>Real-Life Use</b><br/>Hosted Postgres · ORM routes at startups"]
    end

    subgraph future ["Upcoming Modules"]
        direction TB
        U1["<b>Upcoming Module</b><br/>Module 8: Testing Hygiene<br/><i>[Pytest · Review]</i><br/>Automated API checks"]
        U2["<b>Upcoming Module</b><br/>Module 9: Deploy Ops<br/><i>[Docker · CI]</i><br/>Ship the API"]
        U3["<b>Upcoming Module</b><br/>Module 10: Software 3.0<br/><i>[LLM · Prompts]</i><br/>AI features on APIs"]
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

- **Why an ORM helps** backend code talk to a database without raw SQL every time
- How to **define one SQLAlchemy model** that maps to a table
- How FastAPI **connects to Neon** using a database URL
- How to **sketch a simple schema** before you write models
- How one **GET route reads rows** through the ORM

---

## 2. Detailed Explanation

### Why Not Raw SQL in Every Route?

You already created tables on **Neon** and wrote `SELECT` in SQL. That works. Copy-pasting SQL into every FastAPI route does not scale.

An **ORM** (Object-Relational Mapper — a library that maps Python classes to database tables) lets you work with Python objects. The library writes SQL for you.

**Analogy:** SQL is speaking the database’s native language. An ORM is a translator that lets you speak Python.

> **In the Real World:** Teams at **Instagram** (Django ORM) and many FastAPI shops (SQLAlchemy) keep business logic in Python. Recruiters expect you to know both “I can write SQL” and “I can use an ORM.”

**Why It Matters**

- One model change updates how the app talks to the table.
- Python type hints and models reduce typos in column names.
- Swapping hosts (local Postgres vs Neon) is often just a URL change.

**Benefits**

- Faster CRUD once the model exists
- Fewer string-SQL bugs in routes
- Clearer code reviews for beginners

### One-Line Definition

**SQLAlchemy model:** a Python class that describes one table’s columns.

### Messy to Clear

**Messy:** Every route opens a connection and pastes `SELECT * FROM notes WHERE id = 1`.

**Clear:** A `Note` class. A session. `session.get(Note, 1)` inside a GET route.

### Building Blocks

1. **Database URL** — a string Neon gives you (`postgresql://user:pass@host/db`).
2. **Engine** — SQLAlchemy’s connection factory.
3. **Session** — a short-lived “conversation” with the database.
4. **Model** — class ↔ table mapping.
5. **Schema sketch** — boxes and arrows on paper before code.

### Connect FastAPI to Neon

Keep the URL out of Git. Use an environment variable.

```python
import os
from sqlalchemy import create_engine

DATABASE_URL = os.getenv("DATABASE_URL")
engine = create_engine(DATABASE_URL)
```

Your Neon dashboard copies this URL. FastAPI does not “know” Neon. It only knows the URL.

> **In the Real World:** **Vercel**, **Render**, and **Railway** all inject `DATABASE_URL` the same way. Learn the pattern once.

### One Table Model

Imagine a notes app. One table: `notes`.

```python
from sqlalchemy.orm import DeclarativeBase, Mapped, mapped_column
from sqlalchemy import String, Integer

class Base(DeclarativeBase):
    pass

class Note(Base):
    __tablename__ = "notes"
    id: Mapped[int] = mapped_column(Integer, primary_key=True)
    title: Mapped[str] = mapped_column(String(100))
```

This class is the table. `id` is the primary key you already used in SQL.

### Sketch a Simple Schema

Before models, draw:

- One box: `notes`
- Columns: `id`, `title`, `created_at` (optional later)
- One arrow only if a second table appears later

Stay simple. This session is **one table**, not a full ER diagram for a bank.

> **In the Real World:** Product teams at **Notion** still start features with a one-page schema sketch. Code comes second.

### One ORM Read Through GET

Mental flow: HTTP GET → FastAPI route → open session → query `Note` → return JSON.

```python
@app.get("/notes/{note_id}")
def get_note(note_id: int, db: Session):
    note = db.get(Note, note_id)
    return {"id": note.id, "title": note.title}
```

You will wire `Session` properly in class. Predict: if the row is missing, you will later return 404. Today, focus on a successful read.

### Python vs SQL (Same Job)

| SQL | ORM |
|-----|-----|
| `SELECT * FROM notes WHERE id = 1` | `db.get(Note, 1)` |
| Column names as strings | Attributes on `Note` |
| You write every query | SQLAlchemy generates SQL |

You still **need SQL** for joins and debugging. The ORM does not replace Neon knowledge. It sits on top.

### Building Blocks Checklist

- [ ] I can explain ORM in one sentence
- [ ] I can name `engine`, `session`, and `model`
- [ ] I know `DATABASE_URL` must not be committed
- [ ] I can sketch one table with a primary key
- [ ] I can describe GET → session → `db.get`

---

## 3. Practice Exercises

**Exercise 1 — Why ORM**
Write three sentences: what an ORM is, one benefit, and one thing SQL still does better.

**Exercise 2 — Model**
On paper, list fields for a `Task` model: `id`, `title`, `done`. Mark the primary key.

**Exercise 3 — URL**
Without pasting secrets, write the shape of a Neon URL: protocol, user, host, database name.

**Exercise 4 — Schema sketch**
Draw one box for `books` with `id`, `title`, `author`. No second table.

**Exercise 5 — Read path**
Number these steps: open session, receive GET, return JSON, `db.get(Book, 1)`.
