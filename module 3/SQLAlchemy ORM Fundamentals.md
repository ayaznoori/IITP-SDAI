# Lecture Script: SQLAlchemy ORM Fundamentals
**Format:** Facilitator-facing live script | **Duration:** 110 minutes | **Level:** Beginner

---

## Session Flow at a Glance

| Block | Topic | Time |
|---|---|---|
| 1 | Why Does This Matter? | 10 min |
| 2 | SQLAlchemy ORM — Introduction and Role in Backend Development | 15 min |
| 3 | SQLAlchemy Setup and Model Definition | 25 min |
| 4 | Querying Data Using SQLAlchemy ORM | 30 min |
| 5 | Relationship Handling — Managing Relationships Using ORM | 20 min |
| 6 | Lecture Summary and Recap | 10 min |

---

## Block 1 — Why Does This Matter?

> 🎯 **Instructor Note:** This audience has written SQL directly. They know tables, constraints, primary keys, foreign keys, and the four fundamental SQL operations. The hook is not "SQL is hard" — it is "SQL embedded in Python strings is a maintenance and safety problem at scale." Open with the friction, not a sales pitch for ORM. Let the scenario land before you offer the solution. Pause after the opening question.

**[Script:]**

"You have a FastAPI route that creates a user. It looks something like this."

> 🎯 **Instructor Note:** Write this on the board. Keep it rough — the point is the problem, not polished code.

```python
@app.post("/users")
def create_user(user: UserCreate):
    query = f"INSERT INTO users (name, email) VALUES ('{user.name}', '{user.email}')"
    db.execute(query)
```

**[Script:]**

"What is wrong with this? A few things.

The SQL is a raw string. If `user.name` contains a single quote — O'Brien, for example — this query breaks. If a malicious user sends carefully crafted input, they can inject arbitrary SQL. This is SQL injection — one of the most common and destructive security vulnerabilities in web applications. Learners know this from the SQL sessions: string interpolation in queries is dangerous.

Second problem: your application logic is now split across two languages. The Python type system knows nothing about your database schema. You have a `users` table defined in SQL somewhere and a Python dictionary or manual parsing somewhere else. They can drift out of sync. You rename a column in SQL — now every Python string that references that column silently breaks at runtime.

Third: as your application grows, you end up with SQL strings scattered across dozens of route functions, utility files, and scripts. Some use parameterized queries correctly, some do not. Reviewing and refactoring them is painful.

SQLAlchemy ORM solves all three. It maps Python classes directly to database tables. You interact with the database using Python objects and method calls — no raw SQL strings in application code. The ORM generates the SQL, uses parameterized queries always, and ties your Python types to your database schema in one place.

Here is the same operation with SQLAlchemy."

```python
db_user = User(name=user.name, email=user.email)
db.add(db_user)
db.commit()
```

**[Script:]**

"Same result. No SQL strings. No injection risk. The Python class `User` is the single source of truth for what a user record looks like. Everything flows from that definition.

Today we set up SQLAlchemy, define models, query data, and handle relationships — all in Python, all connected to the database you already understand."

---

## Block 2 — SQLAlchemy ORM: Introduction and Role in Backend Development

### 2A — What Is an ORM?

**[Script:]**

"ORM stands for Object-Relational Mapper. It is a layer between your Python application and the relational database that translates between two different worlds: the object-oriented world of Python, where you work with instances of classes, and the relational world of the database, where data lives in rows and columns.

The mapper part is literal. An ORM maps a Python class to a database table. Each attribute of the class maps to a column. Each instance of the class represents one row."

> 🎯 **Instructor Note:** Draw the mapping on the board. Keep it side by side — the class on the left, the table on the right, arrows connecting each attribute to its column. Learners need to see the correspondence before they see any SQLAlchemy syntax.

```
Python Class                    Database Table
─────────────────               ──────────────────────────
class User:                     Table: users
    id: int          ←────────  id      INTEGER PRIMARY KEY
    name: str        ←────────  name    TEXT NOT NULL
    email: str       ←────────  email   TEXT NOT NULL UNIQUE

One instance = one row
user = User(name="Alice", ...)  → | 1 | Alice | alice@example.com |
```

**[Script:]**

"When you create a `User` instance and tell SQLAlchemy to save it, SQLAlchemy generates the INSERT statement, executes it safely with parameterized values, and returns the saved object with its auto-assigned ID. When you query for users, SQLAlchemy generates the SELECT, fetches the rows, and hands you Python objects — not raw tuples.

The SQL still happens. The database still runs SQL. SQLAlchemy generates it for you, correctly, safely, every time."

---

### 2B — SQLAlchemy's Two Layers

**[Script:]**

"SQLAlchemy has two distinct layers. The first is the Core — a low-level SQL expression toolkit. It lets you build SQL queries programmatically using Python objects, but you still think in SQL terms: tables, columns, joins. It is powerful and explicit.

The second is the ORM — built on top of Core. This is what we are using today. You think in Python objects and classes. The ORM translates to Core, which translates to SQL. You rarely need to go below the ORM layer for standard application development.

Knowing both layers exist matters because some SQLAlchemy documentation and examples use Core syntax. When you encounter it, you will recognize that it is a different layer, not a different library."

> 🎯 **Instructor Note:** Do not go deep on Core. Name the distinction once so learners are not confused when they encounter Core examples in documentation, then return to ORM exclusively.

---

### 2C — SQLAlchemy's Role in FastAPI Backend Development

**[Script:]**

"In a FastAPI application, SQLAlchemy plays a specific role in the architecture. FastAPI handles HTTP — routing, validation, serialization. SQLAlchemy handles data persistence — reading and writing to the database. Pydantic models you already know validate the incoming request data and shape the outgoing response. SQLAlchemy models represent the database structure.

These are not the same models. A Pydantic model is for API input and output. A SQLAlchemy model is a database table representation. In practice you define both and map between them in your route functions. This is a common pattern in FastAPI applications and it is intentional — the two concerns are kept separate."

> 🎯 **Instructor Note:** Write this architecture on the board. Learners who do not understand that Pydantic models and SQLAlchemy models are different will make confused design decisions. Make the separation explicit now.

```
HTTP Request
     ↓
FastAPI route
     ↓
Pydantic model  ← validates and shapes API data
     ↓
SQLAlchemy ORM  ← maps to database tables, handles persistence
     ↓
Database (SQLite / PostgreSQL)
```

**Recap of Block 2 before moving on:**

- ORM maps Python classes to database tables; class attributes correspond to columns; instances represent rows
- SQLAlchemy generates SQL from Python operations — it does not replace SQL, it generates it safely
- SQLAlchemy has two layers: Core (SQL expressions) and ORM (object mapping); we use the ORM layer
- Pydantic models handle API validation; SQLAlchemy models handle database structure — they are separate and intentionally so

---

## Block 3 — SQLAlchemy Setup and Model Definition

### 3A — Installation and Engine Setup

**[Script:]**

"SQLAlchemy is installed via pip. For SQLite use, the standard SQLAlchemy package is sufficient. For PostgreSQL in production you add the appropriate database driver — but we are using SQLite today, which requires no additional driver because it is built into Python."

```bash
pip install sqlalchemy
```

**[Script:]**

"The entry point for SQLAlchemy is the engine. The engine is the object that holds the connection configuration — what kind of database, where it lives, how to connect. You create one engine per application, at startup, and reuse it.

The connection string — called a URL in SQLAlchemy — follows a standard format: `dialect://location`. For SQLite, the location is a file path."

> 🎯 **Instructor Note:** Write the URL format on the board with both variants.

```python
# SQLite: file on disk
DATABASE_URL = "sqlite:///./myapp.db"

# SQLite: in memory (for testing, data lost when process ends)
DATABASE_URL = "sqlite:///:memory:"
```

**[Script:]**

"Three slashes for a relative path — `sqlite:///./myapp.db` — the third slash is part of the scheme separator, the fourth is the start of the path. Two slashes for the scheme, one for the path. This trips people up — if SQLite cannot find or create the file, check the slashes.

`connect_args={'check_same_thread': False}` is a SQLite-specific setting. SQLite was designed for single-thread use, but FastAPI can handle multiple concurrent requests across threads. This argument tells SQLite to allow that. PostgreSQL does not need this argument."

**Demo 1 — Engine and Session setup (whiteboard-friendly)**

```python
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker, DeclarativeBase

DATABASE_URL = "sqlite:///./myapp.db"

engine = create_engine(DATABASE_URL,
                       connect_args={"check_same_thread": False})

SessionLocal = sessionmaker(bind=engine, autocommit=False, autoflush=False)

class Base(DeclarativeBase):
    pass
```

**[Script:]**

"Four things defined here, all of them used in everything that follows.

`engine` — the connection to the database. Created once.

`SessionLocal` — a factory for creating sessions. A session is the object you use to run queries and manage transactions. Every request in a FastAPI application gets its own session — opened at the start of the request, closed when it finishes. `autocommit=False` means changes are not saved until you explicitly call `commit()`. `autoflush=False` means SQLAlchemy does not automatically send pending changes to the database before queries — you control when that happens.

`Base` — the declarative base class. All your SQLAlchemy model classes will inherit from this. It is the registration mechanism that lets SQLAlchemy know about your table definitions.

These four lines appear at the top of the database configuration file in virtually every SQLAlchemy application. Recognize them as a unit."

> 🎯 **Instructor Note:** Ask: "Why does each request get its own session rather than sharing one session across the application?" Guide learners to: sessions track state — pending changes, loaded objects. If two requests shared one session, their changes could interfere. A session per request keeps each request's database work isolated.

---

### 3B — Defining a Model

**[Script:]**

"A SQLAlchemy model is a Python class that inherits from `Base`. Class attributes define the columns using `mapped_column` with a SQLAlchemy type. The class name by convention matches the table name — SQLAlchemy derives the table name from the class name by default, or you set it explicitly with `__tablename__`."

> 🎯 **Instructor Note:** Before showing code, revisit the mapping diagram from Block 2. Point to it on the board. Say: "We are now writing the Python side of that mapping." The diagram was not decorative — it is the mental model the code expresses.

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Before the demo, ask: "If I define a `User` model with `__tablename__ = 'users'` and then call `Base.metadata.create_all(engine)`, what does SQLAlchemy do?" Answer: it generates and runs a `CREATE TABLE IF NOT EXISTS users` statement against the database. If the table already exists, nothing changes. If it does not, the table is created with the columns defined in the model.

**Demo 2 — Model definition (whiteboard-friendly)**

```python
from sqlalchemy import String, Integer
from sqlalchemy.orm import Mapped, mapped_column

class User(Base):
    __tablename__ = "users"

    id:    Mapped[int] = mapped_column(Integer, primary_key=True)
    name:  Mapped[str] = mapped_column(String, nullable=False)
    email: Mapped[str] = mapped_column(String, nullable=False, unique=True)

Base.metadata.create_all(engine)
```

**[Script:]**

"Read the class line by line.

`__tablename__ = 'users'` — this class maps to the `users` table.

`id: Mapped[int] = mapped_column(Integer, primary_key=True)` — the `id` column is an integer, is the primary key, and auto-increments. `Mapped[int]` is the Python type annotation. `mapped_column(Integer, primary_key=True)` is the SQLAlchemy column definition. Both sides are needed: the annotation for Python's type system, the column definition for SQLAlchemy's schema generation.

`name` and `email` are both strings. `nullable=False` is the SQLAlchemy equivalent of `NOT NULL`. `unique=True` is the equivalent of `UNIQUE`.

`Base.metadata.create_all(engine)` — this is the line that actually creates the table in the database. It reads all classes registered through `Base`, generates the appropriate `CREATE TABLE` statements, and executes them. Call this once when your application starts."

> 🎯 **Instructor Note:** Common confusion: learners think defining the class creates the table. It does not. The class is a definition. `create_all` is the execution. Separate concept from execution — say this explicitly and let it settle.

**[Script:]**

"The Python type in `Mapped[int]` and the SQLAlchemy type in `mapped_column(Integer, ...)` must be compatible. `Mapped[int]` and `Integer` go together. `Mapped[str]` and `String` go together. `Mapped[bool]` and `Boolean`. The Python annotation is what your code sees. The SQLAlchemy type is what gets written to the database schema."

---

### 3C — The Session as a Unit of Work

**[Script:]**

"Every database operation in SQLAlchemy goes through a session. The session tracks which objects have been loaded, which have been modified, and which are pending. When you call `commit()`, the session sends all pending changes to the database in a single transaction.

In FastAPI, the standard pattern is a dependency that opens a session, yields it to the route, and closes it when the route is done — whether the route succeeded or raised an error."

**Demo 3 — Session dependency for FastAPI (whiteboard-friendly)**

```python
from fastapi import Depends
from sqlalchemy.orm import Session

def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()

# Usage in a route
@app.post("/users")
def create_user(name: str, email: str, db: Session = Depends(get_db)):
    user = User(name=name, email=email)
    db.add(user)
    db.commit()
    db.refresh(user)
    return {"id": user.id, "name": user.name}
```

**[Script:]**

"The `get_db` function is a FastAPI dependency. `yield db` hands the session to the route. The `finally` block ensures the session is closed regardless of what happens in the route — success, exception, anything. The session is cleaned up.

Inside the route: create a `User` instance — not saved yet. `db.add(user)` — registered with the session, not saved yet. `db.commit()` — sends the INSERT to the database, commits the transaction. `db.refresh(user)` — reloads the object from the database, which fills in auto-generated values like `id`. After `refresh`, `user.id` has its actual database-assigned value.

The sequence `add → commit → refresh` is the standard create pattern in SQLAlchemy. Memorize it as a unit."

> 🎯 **Instructor Note:** Ask: "Why do we need `db.refresh(user)` after `db.commit()`?" Guide learners to: after commit, the local Python object may be expired — SQLAlchemy does not automatically re-read from the database. `refresh` forces a SELECT to reload the object's current state, including any values the database generated, like the auto-incremented `id`. Without refresh, accessing `user.id` after commit on some configurations raises a lazy-load error.

**Recap of Block 3 before moving on:**

- The engine holds connection configuration; created once per application with a connection URL
- `SessionLocal` is a session factory; each request gets its own session instance
- `Base` is the declarative base; all model classes inherit from it and register their table definitions
- A model class defines `__tablename__` and columns using `Mapped[type]` and `mapped_column(...)`
- `Base.metadata.create_all(engine)` generates and executes `CREATE TABLE` statements — the class definition alone does not create the table
- The `get_db` dependency opens a session, yields it to the route, and closes it in `finally`
- The standard create sequence: `db.add(obj)` → `db.commit()` → `db.refresh(obj)`

---

## Block 4 — Querying Data Using SQLAlchemy ORM

### 4A — The Query Interface

**[Script:]**

"SQLAlchemy ORM provides a query interface that generates SQL from Python method calls. You build a query by starting with `db.query(Model)` — this is roughly equivalent to `SELECT * FROM table`. Then you chain filtering, ordering, and limiting methods. Finally you call a terminator method — `all()`, `first()`, `one()`, or `count()` — to execute the query and get results.

Nothing is sent to the database until you call the terminator. Every method before it just builds the query object. This is important: you can build a partial query, pass it to a helper function to add more filters, and execute it once at the end."

> 🎯 **Instructor Note:** Write the query-building pattern on the board as a progression, not as one line.

```python
query = db.query(User)           # SELECT * FROM users
query = query.filter(...)        # WHERE ...
query = query.order_by(...)      # ORDER BY ...
query = query.limit(10)          # LIMIT 10
results = query.all()            # Execute — returns a list
```

---

### 4B — Retrieving All Records and by Primary Key

**[Script:]**

"The two most basic queries: get everything, get one specific record by ID."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If the users table has three rows and I call `db.query(User).all()`, what type does Python give me back?" Answer: a list of `User` instances — Python objects, not tuples, not dictionaries. Each object has `.id`, `.name`, `.email` attributes. This is the ORM value proposition — the result is native Python objects.

**Demo 4 — Basic retrieval (whiteboard-friendly)**

```python
# All users — returns a list of User instances
all_users = db.query(User).all()
for user in all_users:
    print(user.name, user.email)

# One user by primary key — returns one User or None
user = db.get(User, 1)
if user:
    print(user.name)
```

**[Script:]**

"`db.query(User).all()` generates `SELECT * FROM users` and returns a list of `User` objects. Iterating the list gives you individual `User` instances with attribute access on each.

`db.get(User, 1)` is the idiomatic way to retrieve a single record by primary key. It generates `SELECT * FROM users WHERE id = 1` and returns one `User` object, or `None` if no row with that ID exists. Checking for `None` before accessing attributes is essential — do not assume a record exists."

---

### 4C — Filtering with filter() and filter_by()

**[Script:]**

"SQLAlchemy provides two filtering methods. `filter()` takes SQLAlchemy column expressions — these look like Python comparisons but use the model's column attributes. `filter_by()` takes keyword arguments with plain Python values — simpler syntax for equality checks.

The expressions in `filter()` use the model class attributes directly. `User.email == 'alice@example.com'` is not a Python equality check — SQLAlchemy overrides the `==` operator on column attributes to produce a SQL expression. The result is a condition object that SQLAlchemy translates to SQL."

> 🎯 **Instructor Note:** This operator overloading surprises learners. Address it directly before the demo.

**[Script:]**

"When you write `User.email == 'alice@example.com'` in a `filter()` call, Python is not evaluating whether those are equal — the `==` operator on a SQLAlchemy column attribute returns an expression object. SQLAlchemy collects that object and uses it to generate the WHERE clause. This is one of the places where SQLAlchemy looks like Python but is doing something more specific underneath."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Before the demo, ask: "If I call `db.query(User).filter(User.age > 25).all()` and the users table has Alice at 28, Bob at 34, and Carol at 22, what does Python give me back?" Answer: a list of two `User` objects — Alice and Bob. Carol at 22 does not satisfy `age > 25`.

**Demo 5 — filter() and filter_by() (whiteboard-friendly)**

```python
# filter() with column expressions
alice = db.query(User).filter(User.email == "alice@example.com").first()

# Multiple conditions — chain filter() or use comma-separation
older_users = db.query(User).filter(User.age > 25, User.age < 40).all()

# filter_by() for simple equality
bob = db.query(User).filter_by(name="Bob").first()
```

**[Script:]**

"First query: returns the first matching `User` or `None`. `.first()` is the terminator — it adds `LIMIT 1` to the generated SQL and returns a single object.

Second query: multiple conditions passed as separate arguments to `filter()` are combined with AND. Same as `WHERE age > 25 AND age < 40`.

Third query: `filter_by(name='Bob')` is shorthand for `filter(User.name == 'Bob')`. Use `filter_by` for simple equality, `filter()` for everything else — comparisons, inequalities, OR conditions, NULL checks."

> 🎯 **Instructor Note:** Ask: "How do you write an OR condition in filter()?" Answer: import `or_` from `sqlalchemy` and use `filter(or_(User.age < 25, User.name == 'Carol'))`. Do not go deep — name it and move on. The point is that `filter()` handles all complex cases and `filter_by()` is the simple equality shortcut.

---

### 4D — Ordering, Limiting, and Counting

**[Script:]**

"Three more query methods you will use constantly. `order_by()` sorts the result. `limit()` caps the number of rows. `count()` returns the number of matching rows without fetching the objects."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "What SQL does `db.query(User).order_by(User.age.desc()).limit(1).first()` generate?" Answer: `SELECT * FROM users ORDER BY age DESC LIMIT 1`. It returns the oldest user — one object or None.

**Demo 6 — order_by, limit, count (whiteboard-friendly)**

```python
# Sort descending by age, get first result
oldest = db.query(User).order_by(User.age.desc()).first()

# Get first 5 users ordered by name
page = db.query(User).order_by(User.name).limit(5).all()

# Count matching rows without fetching objects
total = db.query(User).filter(User.age > 25).count()
print(total)
```

**[Script:]**

"`.desc()` is called on the column attribute to get descending order. `.asc()` for ascending — though ascending is the default and can be omitted.

`.count()` generates `SELECT COUNT(*) FROM users WHERE age > 25`. It returns an integer — not a list, not an object. When you only need to know how many records match, use `count()` instead of `all()` and measuring `len()` — `count()` does the counting in the database, not in Python after fetching all the rows."

---

### 4E — Updating and Deleting Records

**[Script:]**

"To update a record, fetch it, modify its attributes, and commit. SQLAlchemy detects that the object has changed — it tracks the before and after state — and generates the appropriate UPDATE statement on commit.

To delete a record, fetch it and call `db.delete(obj)`, then commit."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If I fetch a User, change `user.email` to a new value, and call `db.commit()` — without calling `db.add(user)` — does the change get saved?" Answer: yes. SQLAlchemy tracks objects that are already associated with the session. You do not need to re-add them. `add()` is only for new objects not yet associated with the session.

**Demo 7 — Update and Delete (whiteboard-friendly)**

```python
# Update
user = db.get(User, 1)
if user:
    user.email = "newalice@example.com"
    db.commit()
    db.refresh(user)

# Delete
user = db.get(User, 1)
if user:
    db.delete(user)
    db.commit()
```

**[Script:]**

"Update pattern: get the object, change the attribute, commit. SQLAlchemy sees the change and generates `UPDATE users SET email = ? WHERE id = 1` with a parameterized value. No SQL string. No injection risk.

Delete pattern: get the object, call `db.delete(obj)`, commit. SQLAlchemy generates `DELETE FROM users WHERE id = 1`. After commit, the row is gone and the Python object is detached from the session.

Always check for `None` before operating on a fetched object. If `db.get(User, 1)` returns `None`, the object does not exist. Calling `.email` on `None` raises an `AttributeError`."

**Recap of Block 4 before moving on:**

- `db.query(Model)` starts a query; chain `.filter()`, `.order_by()`, `.limit()` to build it; terminate with `.all()`, `.first()`, `.count()`
- Nothing is sent to the database until the terminator is called
- `db.get(Model, pk)` retrieves by primary key and returns `None` if not found — always check for `None`
- `filter()` takes column expressions using model attributes with overloaded operators; `filter_by()` is shorthand for equality
- Multiple conditions in `filter()` are combined with AND
- Update: fetch, modify attribute, commit — no re-add needed for existing tracked objects
- Delete: fetch, `db.delete(obj)`, commit
- `count()` counts in the database; prefer it over fetching all rows and measuring `len()`

---

## Block 5 — Relationship Handling: Managing Relationships Using ORM

### 5A — What ORM Relationships Replace

**[Script:]**

"In raw SQL, you defined a foreign key column, wrote JOIN queries to combine data from two tables, and assembled the results in Python. In SQLAlchemy ORM, you define the relationship in Python and access related objects through attribute navigation — no manual JOIN queries in your route code.

SQLAlchemy generates the JOIN. You access related data as if it were a Python attribute."

> 🎯 **Instructor Note:** Write the before and after on the board. The contrast makes the value concrete.

```python
# Raw SQL approach — manual JOIN
query = "SELECT u.name, o.item FROM users u JOIN orders o ON u.id = o.user_id WHERE u.id = 1"
rows = db.execute(query).fetchall()
orders = [row[1] for row in rows]

# SQLAlchemy relationship — attribute access
user = db.get(User, 1)
orders = user.orders   # SQLAlchemy generates the query automatically
```

---

### 5B — Defining a One-to-Many Relationship

**[Script:]**

"A SQLAlchemy relationship is defined using `relationship()` from `sqlalchemy.orm`. You define it on the parent model — the one side of one-to-many — as an attribute. It tells SQLAlchemy: when I access this attribute, go find the related rows in the child table.

The child model — the many side — keeps the actual foreign key column. The relationship attribute on the parent is not a real column in the database. It is a Python-side navigation property. SQLAlchemy manages what happens when you access it."

**Demo 8 — One-to-many relationship definition (whiteboard-friendly)**

```python
from sqlalchemy.orm import relationship
from sqlalchemy import ForeignKey

class User(Base):
    __tablename__ = "users"
    id:     Mapped[int] = mapped_column(Integer, primary_key=True)
    name:   Mapped[str] = mapped_column(String, nullable=False)
    email:  Mapped[str] = mapped_column(String, nullable=False, unique=True)
    orders: Mapped[list] = relationship("Order", back_populates="user")

class Order(Base):
    __tablename__ = "orders"
    id:      Mapped[int] = mapped_column(Integer, primary_key=True)
    item:    Mapped[str] = mapped_column(String, nullable=False)
    user_id: Mapped[int] = mapped_column(Integer, ForeignKey("users.id"))
    user:    Mapped["User"] = relationship("User", back_populates="orders")
```

**[Script:]**

"Read the `User` model. `orders` is a relationship attribute — not a column. It is a list of `Order` objects belonging to this user. `relationship('Order', back_populates='user')` — the first argument is the class name of the related model as a string, `back_populates` names the corresponding attribute on the other side.

Read the `Order` model. `user_id` is the actual foreign key column — this is the real column in the database, referencing `users.id`. `user` is the relationship attribute — a single `User` object, because each order belongs to one user. `back_populates='orders'` names the corresponding attribute on `User`.

`back_populates` keeps both sides synchronized. If you add an order to `user.orders`, SQLAlchemy automatically sets `order.user` and `order.user_id`. The two attributes are mirrors of each other, maintained by SQLAlchemy."

> 🎯 **Instructor Note:** Ask: "Where is the foreign key column? Is it on User or on Order?" Answer: on `Order` — `user_id`. This is identical to the SQL rule from the database session: the foreign key lives on the many side. The relationship attribute on `User` is not a column — it is a Python navigation shortcut. The column that actually links the tables is `user_id` on `Order`.

---

### 5C — Using Relationships in Practice

**[Script:]**

"Once the relationship is defined, accessing related data is attribute access on the object. SQLAlchemy issues the necessary query when you first access the attribute."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Before the demo, ask: "If I load a `User` object with `db.get(User, 1)` and then access `user.orders`, when does SQLAlchemy hit the database?" Answer: when you access `user.orders` for the first time. The initial `db.get` fetches only the user row. The orders are loaded on demand — this is called lazy loading.

**Demo 9 — Accessing relationships (whiteboard-friendly)**

```python
# Creating related objects
user = User(name="Alice", email="alice@example.com")
db.add(user)
db.commit()
db.refresh(user)

order = Order(item="Keyboard", user_id=user.id)
db.add(order)
db.commit()

# Navigating the relationship
user = db.get(User, user.id)
print(user.orders)          # List of Order objects for this user

order = db.get(Order, 1)
print(order.user.name)      # Navigate from order back to user
```

**[Script:]**

"Create the user, commit to get the ID, then create the order with that `user_id`. After commit, the relationship is in the database.

Now navigate: `user.orders` returns a list of `Order` objects. No query written. SQLAlchemy generates `SELECT * FROM orders WHERE user_id = 1` when you access that attribute.

`order.user.name` navigates the reverse direction — from an order back to its user. SQLAlchemy generates `SELECT * FROM users WHERE id = ?` using the stored `user_id`. You get a `User` object and access its `.name` attribute.

This is the practical value of ORM relationships: traversal through Python attribute access rather than writing JOIN queries in every route function."

---

### 5D — Lazy Loading and Eager Loading: Knowing the Difference

**[Script:]**

"By default, SQLAlchemy uses lazy loading. Related objects are loaded from the database the first time you access the relationship attribute — not when you load the parent object. This is efficient when you often do not need the related data. It is a problem when you load a list of fifty users and then access `user.orders` on each one — that generates fifty additional queries, one per user. This is called the N+1 problem.

The fix is eager loading — tell SQLAlchemy to load the related objects in the same query as the parent."

> 🎯 **Instructor Note:** Name the N+1 problem explicitly. Write it on the board. Learners will encounter it. Knowing its name means they can search for it, recognize it in profiling output, and understand why the solution works.

```
N+1 Problem:
1 query to load N users
+ N queries to load orders for each user
= N+1 queries total

With 100 users: 101 database round trips
```

**[Script:]**

"Eager loading solves this with `joinedload` — SQLAlchemy fetches the parent and related objects in a single JOIN query."

```python
from sqlalchemy.orm import joinedload

users = db.query(User).options(joinedload(User.orders)).all()
for user in users:
    print(user.name, user.orders)   # No extra queries — already loaded
```

**[Script:]**

"`joinedload(User.orders)` tells SQLAlchemy: when you load users, immediately also load their orders using a JOIN. One query, all the data. For a list of fifty users, this is one query instead of fifty-one.

You do not need to use eager loading everywhere — only when you know you will access the relationship on every object in a collection. For single-object lookups where you may or may not need the related data, lazy loading is fine."

> 🎯 **Instructor Note:** Do not go deep on all loading strategies — there are several (lazy, eager, subquery, select). Name two: lazy (default, load on access) and eager (joinedload, load with join). That is sufficient for beginners. If learners ask about others, acknowledge they exist and move on.

**Recap of Block 5 before moving on:**

- ORM relationships replace manual JOIN queries with Python attribute navigation
- Define `relationship()` on the parent model; the foreign key column stays on the child model — same rule as SQL
- `back_populates` links the two sides of the relationship and keeps them synchronized
- Accessing a relationship attribute triggers a database query — this is lazy loading, the default
- The N+1 problem occurs when you access a relationship attribute on each item in a list — one query per item
- Use `joinedload` to eager-load related objects in the same query as the parent, eliminating N+1

---

## Block 6 — Lecture Summary

> 🎯 **Instructor Note:** Deliver as active recall. Ask before confirming each point. "What does ORM stand for? What does `Base.metadata.create_all(engine)` do? What is the standard create sequence? What is the difference between `filter()` and `filter_by()`? Where does the foreign key column live in a one-to-many ORM relationship? What is the N+1 problem?" Keep pace brisk — this is consolidation.

**SQLAlchemy ORM — Introduction and Role in Backend Development**

- ORM — Object-Relational Mapper — translates between Python objects and database rows; a Python class maps to a table, an instance maps to a row
- SQLAlchemy generates SQL from Python operations; raw SQL strings in application code are replaced by method calls that produce parameterized queries, eliminating SQL injection risk
- SQLAlchemy has two layers: Core (SQL expressions) and ORM (object mapping); the ORM layer is used for standard application development
- Pydantic models handle API request and response data; SQLAlchemy models represent database structure — they are separate and serve different purposes

**SQLAlchemy Setup and Model Definition**

- `create_engine(DATABASE_URL)` creates the connection configuration; for SQLite add `connect_args={"check_same_thread": False}`
- `SessionLocal = sessionmaker(bind=engine, autocommit=False, autoflush=False)` creates a session factory; each request gets its own session
- `class Base(DeclarativeBase): pass` is the registration base; all model classes inherit from it
- A model class defines `__tablename__` and column attributes using `Mapped[type]` and `mapped_column(...)`
- `Base.metadata.create_all(engine)` executes `CREATE TABLE` statements — the class definition alone does not create the table
- The `get_db` FastAPI dependency opens a session, yields it, and closes it in `finally` — session-per-request is the standard pattern
- Standard create sequence: `db.add(obj)` → `db.commit()` → `db.refresh(obj)`

**Querying Data Using SQLAlchemy ORM**

- `db.query(Model)` starts a query; chain `.filter()`, `.order_by()`, `.limit()`; terminate with `.all()`, `.first()`, or `.count()`
- No SQL is sent to the database until the terminator method is called
- `db.get(Model, pk)` retrieves by primary key; returns `None` if not found — always check before use
- `filter()` uses column expressions with overloaded operators (`User.age > 25`); `filter_by()` is shorthand for equality checks
- Multiple conditions in `filter()` are combined with AND; pass them as separate arguments
- Update: fetch the object, modify its attribute, commit — re-add is not needed for already-tracked objects
- Delete: fetch the object, `db.delete(obj)`, commit
- `count()` counts rows in the database; prefer it over `len(all())` for large tables

**Relationship Handling — Managing Relationships Using ORM**

- `relationship()` on a model class creates a Python navigation attribute; the actual foreign key column stays on the child model
- `back_populates` links both sides of the relationship and keeps them synchronized
- Accessing a relationship attribute for the first time triggers a database query — this is lazy loading, the SQLAlchemy default
- The N+1 problem: loading a list of N objects and accessing a relationship on each generates N additional queries
- `joinedload(Model.relationship)` in `.options()` eager-loads related objects in the same query, solving N+1
- Relationship traversal is bidirectional: navigate from parent to children through the list attribute, from child to parent through the single-object attribute

**Why All of This Matters Together**

- Writing SQL as raw strings in Python application code is a security risk, a maintenance burden, and a source of schema drift — SQLAlchemy ORM addresses all three by making the Python class the single source of truth for the database schema, generating parameterized SQL automatically, and expressing database operations as Python method calls that the type system can reason about
- The four capabilities covered — setup and model definition, querying, updating and deleting, and relationship navigation — are everything needed to build the persistence layer of a real FastAPI backend; every route that reads, writes, modifies, or relates data goes through exactly these patterns

---

*End of script.*