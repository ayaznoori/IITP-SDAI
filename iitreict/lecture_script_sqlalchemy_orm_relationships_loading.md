# Lecture Script: SQLAlchemy ORM Fundamentals
**Format:** Facilitator-facing live script | **Duration:** 110 minutes | **Level:** Beginner

---

## Session Flow at a Glance

| Block | Topic | Time |
|---|---|---|
| 1 | Why Does This Matter? | 10 min |
| 2 | SQLAlchemy ORM Introduction | 20 min |
| 3 | Defining Models and Relationships | 25 min |
| 4 | One-to-Many and Many-to-Many Relationships | 35 min |
| 5 | Querying with SQLAlchemy | 15 min |
| 6 | Lecture Summary and Recap | 5 min |

---

## Block 1 — Why Does This Matter?

> 🎯 **Instructor Note:** This audience already writes raw SQL with joins, knows foreign keys, and has connected Python to a database directly. The hook should highlight the specific friction of doing relationship work — joins, multi-table inserts — by hand in Python, not re-explain what a database is. Wait after the opening question.

**[Script:]**

"You can already write a JOIN in SQL and you can connect Python to a database and execute queries. Here is the gap that remains.

Picture a route that creates an order and needs to also confirm the user it belongs to exists, then return the user's name alongside the order details. In raw SQL and a Python driver, that means: write a SELECT to check the user exists, write an INSERT for the order, write a JOIN query to fetch the combined result, and manually convert each row tuple into something your API can return as JSON. Four separate pieces of SQL, with you tracking the relationships between them by hand every time.

Now imagine a many-to-many situation — students and courses, or products and tags. Every time you want to enroll a student in a course, you write an INSERT into a junction table. Every time you want a list of a student's courses, you write a JOIN through that junction table. Every one of these is correct SQL — and every one of these you already know how to write. The problem is the repetition and the manual bookkeeping multiplies as your application grows.

SQLAlchemy ORM lets you define these relationships once, as Python code, and then navigate them as object attributes. `student.courses` gives you the list. `order.user.name` gives you the name. No JOIN written by hand in the route. No manual row-tuple conversion. The relationship is defined once; every place in your application that needs it just uses the attribute.

Today: how the ORM maps classes to tables, how to define one-to-many and many-to-many relationships in Python, and how to query with the ORM's interface once those relationships exist."

---

## Block 2 — SQLAlchemy ORM Introduction

### 2A — What an ORM Does

**[Script:]**

"ORM stands for Object-Relational Mapper. It is the layer that translates between Python's object-oriented world — classes and instances — and the relational database's world — tables and rows. A Python class maps to a table. Each instance of that class represents one row."

> 🎯 **Instructor Note:** Draw this mapping on the board before any SQLAlchemy syntax appears.

```
Python Class                    Database Table
─────────────────               ──────────────────────────
class User:                     Table: users
    id: int          ←────────  id      INTEGER PRIMARY KEY
    name: str        ←────────  name    TEXT NOT NULL
    email: str       ←────────  email   TEXT NOT NULL UNIQUE

One instance = one row
```

**[Script:]**

"When you create a `User` instance and save it, SQLAlchemy generates the INSERT, executes it with parameterized values — the same SQL injection protection as parameterized queries you would write directly — and returns the object with its database-assigned ID filled in. When you query, SQLAlchemy generates the SELECT and hands you back Python objects, not raw tuples.

The database still runs SQL underneath all of this. SQLAlchemy is generating that SQL correctly and safely, every time, so you do not write it by hand for routine operations."

> 🎯 **Instructor Note:** Ask: "What would the equivalent of creating and saving a User instance have looked like in raw SQL?" Answer: an INSERT statement with parameterized placeholders for name and email, executed through a cursor. Confirming this out loud establishes that SQLAlchemy is automating something learners already know how to do by hand — not replacing it with something unfamiliar.

---

### 2B — Engine, Session, and Base

**[Script:]**

"Three pieces of setup appear at the top of every SQLAlchemy application. The engine holds the database connection configuration. The session is what you use to run queries and save changes. The declarative base is the class that all your model classes inherit from, which registers them with SQLAlchemy."

```python
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker, DeclarativeBase

engine = create_engine("sqlite:///./myapp.db",
                       connect_args={"check_same_thread": False})

SessionLocal = sessionmaker(bind=engine, autocommit=False, autoflush=False)

class Base(DeclarativeBase):
    pass
```

**[Script:]**

"`connect_args={'check_same_thread': False}` is SQLite-specific — it allows the connection to be used across the multiple threads that a web framework like FastAPI may use. `autocommit=False` means nothing is saved until you explicitly call `commit()` — the same discipline as calling `commit()` directly on a database connection.

These three lines — engine, session factory, declarative base — are infrastructure you write once and reuse throughout the application."

**Recap of Block 2 before moving on:**

- ORM maps Python classes to database tables; instances represent rows
- SQLAlchemy generates parameterized SQL automatically — the same injection protection as writing parameterized queries by hand
- The engine holds connection configuration; the session manages queries and saved changes; the declarative base registers model classes

---

## Block 3 — Defining Models and Relationships

### 3A — Defining a Model

**[Script:]**

"A model is a Python class inheriting from `Base`. `__tablename__` names the table. Each column is declared with a type annotation and `mapped_column`."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "After I define this class, does the `users` table exist in the database yet?" Answer: not until `Base.metadata.create_all(engine)` runs. The class is a definition; `create_all` is the execution that generates and runs the `CREATE TABLE` statement.

**Demo 1 — Defining a model (whiteboard-friendly)**

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

"`Mapped[int]` is the Python-facing type annotation. `mapped_column(Integer, primary_key=True)` is the SQLAlchemy column definition that determines what gets written to the database schema. `nullable=False` is the ORM equivalent of `NOT NULL`. `unique=True` is the equivalent of `UNIQUE` — both constraints you already know from raw SQL, expressed here as Python keyword arguments instead of SQL keywords.

`Base.metadata.create_all(engine)` is the line that actually creates the table — it reads every class registered through `Base` and runs the corresponding `CREATE TABLE` statements."

> 🎯 **Instructor Note:** Common confusion point: learners think defining the class creates the table. Say explicitly: "Definition and execution are two separate steps in SQLAlchemy, just as `CREATE TABLE` had to actually run as a statement in raw SQL — writing the schema on paper does not create it in the database."

---

### 3B — Introducing the relationship() Construct

**[Script:]**

"Relationships connect two model classes the way a foreign key connects two tables — except in Python, the connection becomes a navigable attribute. You import `relationship` from `sqlalchemy.orm` and declare it on a model class.

The relationship attribute is not a real column in the database. It is a Python-side instruction: when this attribute is accessed, go fetch the related rows using the foreign key. The actual foreign key column lives on whichever table holds it in the schema — same as in raw SQL, the foreign key always lives on the many side of a one-to-many relationship."

> 🎯 **Instructor Note:** This is the conceptual bridge for the entire block. State it clearly: the relationship() call does not change the database schema — `mapped_column` and `ForeignKey` do that. `relationship()` only creates the Python navigation. Two different mechanisms, often appearing in the same class, and the distinction matters once learners start debugging why a relationship is not behaving as expected.

**Recap of Block 3 before moving on:**

- A model class inherits from `Base`, declares `__tablename__`, and defines columns with `Mapped[type]` and `mapped_column(...)`
- `nullable=False` and `unique=True` are the ORM equivalents of `NOT NULL` and `UNIQUE`
- `Base.metadata.create_all(engine)` executes the `CREATE TABLE` statements; defining the class alone does not create the table
- `relationship()` creates a Python-side navigable attribute; the actual foreign key column is defined separately with `mapped_column` and `ForeignKey`

---

## Block 4 — One-to-Many and Many-to-Many Relationships

### 4A — One-to-Many: Foreign Key Plus relationship()

**[Script:]**

"One user can have many orders. In SQL you already know this means a `user_id` foreign key column on the orders table. In SQLAlchemy, you write that foreign key column exactly as before, and then add a `relationship()` on each side so both directions are navigable in Python."

**Demo 2 — One-to-many relationship (whiteboard-friendly)**

```python
from sqlalchemy.orm import relationship
from sqlalchemy import ForeignKey

class User(Base):
    __tablename__ = "users"
    id:     Mapped[int] = mapped_column(Integer, primary_key=True)
    name:   Mapped[str] = mapped_column(String, nullable=False)
    orders: Mapped[list] = relationship("Order", back_populates="user")

class Order(Base):
    __tablename__ = "orders"
    id:      Mapped[int] = mapped_column(Integer, primary_key=True)
    item:    Mapped[str] = mapped_column(String, nullable=False)
    user_id: Mapped[int] = mapped_column(Integer, ForeignKey("users.id"))
    user:    Mapped["User"] = relationship("User", back_populates="orders")
```

**[Script:]**

"`user_id` on `Order` is the real foreign key column — this is what is actually written to the database, identical in purpose to the `FOREIGN KEY (user_id) REFERENCES users(id)` you wrote in raw SQL.

`orders` on `User` is a list — one user can have many orders. `user` on `Order` is a single object — one order belongs to one user. `back_populates` names the corresponding attribute on the other side and keeps both synchronized: set one side, SQLAlchemy keeps the other side consistent."

> 🎯 **Instructor Note:** Ask: "Where is the foreign key column? Is it on User or on Order?" Answer: on `Order` — `user_id`. This is identical to the SQL rule already known: the foreign key lives on the many side. The relationship attribute on `User` is not a column — it is a Python navigation shortcut.

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If I load a `User` object and access `user.orders`, what type of Python object do I get back?" Answer: a list of `Order` objects. Even if the user has zero orders, you get an empty list, never `None` — that distinction matters when learners write conditional logic against it.

**Demo 3 — Using a one-to-many relationship (whiteboard-friendly)**

```python
user = User(name="Alice")
db.add(user)
db.commit()

order = Order(item="Keyboard", user_id=user.id)
db.add(order)
db.commit()

print(user.orders)        # list of Order objects
print(order.user.name)    # "Alice" — navigating the other direction
```

**[Script:]**

"Create the user, commit so the database assigns an ID, create the order using that ID. Then navigate both directions: `user.orders` gives the list, `order.user.name` gives the name on the other side — no JOIN written by hand."

---

### 4B — Many-to-Many: The Junction Table in SQLAlchemy

**[Script:]**

"Many-to-many is where the SQL pattern you already know — a junction table with two foreign keys — gets expressed slightly differently in SQLAlchemy. Instead of defining the junction table as a full model class, you typically define it as a plain `Table` object, and SQLAlchemy uses it automatically behind the relationship.

Think of students and courses. One student can take many courses, one course can have many students. Neither side is the 'many' side alone — both are. The junction table records which student-course pairs exist, exactly as it did in raw SQL."

> 🎯 **Instructor Note:** Draw the three-table structure on the board exactly as it would look in SQL, before showing the Python. The visual continuity with what learners already know from SQL makes the SQLAlchemy syntax much easier to absorb.

```
students                   enrollments (junction)       courses
+----+-------+             +------------+-----------+   +----+----------+
| id | name  |             | student_id | course_id |   | id | title    |
+----+-------+             +------------+-----------+   +----+----------+
| 1  | Alice |  <--------- | 1          | 1         |   | 1  | Math     |
+----+-------+             +------------+-----------+   +----+----------+
```

**Demo 4 — Many-to-many relationship (whiteboard-friendly)**

```python
from sqlalchemy import Table, Column

enrollments = Table(
    "enrollments", Base.metadata,
    Column("student_id", ForeignKey("students.id"), primary_key=True),
    Column("course_id",  ForeignKey("courses.id"),  primary_key=True),
)

class Student(Base):
    __tablename__ = "students"
    id:      Mapped[int] = mapped_column(Integer, primary_key=True)
    name:    Mapped[str] = mapped_column(String, nullable=False)
    courses: Mapped[list] = relationship("Course", secondary=enrollments,
                                          back_populates="students")

class Course(Base):
    __tablename__ = "courses"
    id:       Mapped[int] = mapped_column(Integer, primary_key=True)
    title:    Mapped[str] = mapped_column(String, nullable=False)
    students: Mapped[list] = relationship("Student", secondary=enrollments,
                                           back_populates="courses")
```

**[Script:]**

"`enrollments` is a plain `Table` object, not a model class — it has no Python-side behavior of its own, exactly like the junction table in raw SQL having no real purpose beyond recording the link. Its two columns are both foreign keys and together form a composite primary key, written here as two `primary_key=True` columns — identical to what you wrote directly in SQL as `PRIMARY KEY (student_id, course_id)`.

The key new piece is `secondary=enrollments` in the `relationship()` call. This tells SQLAlchemy: this relationship goes through a junction table, not a direct foreign key. Both `Student.courses` and `Course.students` are lists, because the relationship is symmetric — many to many, both directions are 'many'."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If I add a course to `student.courses`, what does SQLAlchemy do behind the scenes? Do I need to manually insert into the enrollments table?" Answer: SQLAlchemy automatically inserts the corresponding row into the `enrollments` table when you commit. You never write an INSERT into the junction table directly when using the ORM relationship.

**Demo 5 — Using a many-to-many relationship (whiteboard-friendly)**

```python
alice = Student(name="Alice")
math = Course(title="Math")

alice.courses.append(math)

db.add(alice)
db.commit()

print(alice.courses)       # [Course(title="Math")]
print(math.students)       # [Student(name="Alice")]
```

**[Script:]**

"`alice.courses.append(math)` — appending to a Python list — is the entire enrollment operation. On `commit()`, SQLAlchemy inserts the corresponding row into `enrollments` automatically. No manual INSERT into the junction table. And because of `back_populates`, `math.students` already reflects Alice without an additional query needed on your part to keep them in sync."

> 🎯 **Instructor Note:** Pause here. Ask learners to compare: "What would the equivalent raw SQL have been for this enrollment?" Let them say it: an INSERT into the enrollments table with Alice's id and Math's id. Confirming this out loud reinforces that the ORM is not doing anything magical — it is automating a specific SQL statement they already know how to write by hand.

**Recap of Block 4 before moving on:**

- One-to-many: the foreign key column lives on the many side, defined with `mapped_column` and `ForeignKey`; `relationship()` on each side makes both directions navigable
- `back_populates` keeps both sides of a relationship synchronized automatically
- Many-to-many: a junction table is defined as a plain `Table` object with two foreign key columns forming a composite primary key
- `secondary=junction_table` in `relationship()` tells SQLAlchemy the relationship goes through a junction table
- Appending to a relationship list and committing automatically inserts the corresponding junction table row — no manual INSERT required

---

## Block 5 — Querying with SQLAlchemy

### 5A — The Query Interface

**[Script:]**

"`db.query(Model)` starts a query, roughly equivalent to `SELECT * FROM table`. You chain `.filter()`, `.order_by()`, `.limit()` to build it, and call a terminator — `.all()`, `.first()`, or `.count()` — to actually execute it and get results. Nothing runs against the database until the terminator is called."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If a table has three rows and I call `db.query(User).all()`, what does Python give back?" Answer: a list of three `User` objects, not tuples, not dictionaries — full Python objects with attribute access.

**Demo 6 — Basic queries (whiteboard-friendly)**

```python
all_users = db.query(User).all()

alice = db.query(User).filter(User.name == "Alice").first()

count = db.query(User).filter(User.name == "Alice").count()
```

**[Script:]**

"`filter()` takes column expressions — `User.name == 'Alice'` is not a Python equality check, it is SQLAlchemy's overloaded `==` operator producing a SQL condition object. `.first()` returns one object or `None`. `.count()` returns an integer computed in the database, not by fetching every row and measuring `len()`.

`db.get(User, 1)` is the idiomatic shortcut for retrieving by primary key — equivalent to `filter(User.id == 1).first()` but more direct."

> 🎯 **Instructor Note:** This operator overloading surprises learners. Address it directly: "When you write `User.name == 'Alice'` inside `filter()`, Python is not evaluating that comparison right then — SQLAlchemy has overridden `==` on column attributes to produce an expression object that gets translated into the SQL WHERE clause." Naming this prevents confusion later when learners see expressions that look like plain Python but behave differently.

---

### 5B — Querying Across Relationships

**[Script:]**

"Once relationships are defined, you do not need to manually write JOIN-based filters for the common case — you navigate the relationship attribute directly. But you can still use `filter()` and `join()` together when you need to filter based on a related table's columns."

**Demo 7 — Filtering through a relationship (whiteboard-friendly)**

```python
# Direct attribute navigation — no JOIN written by hand
user = db.get(User, 1)
orders = user.orders

# Explicit join when filtering by a related table's column
results = (db.query(Order)
             .join(User)
             .filter(User.name == "Alice")
             .all())
```

**[Script:]**

"The first approach uses the relationship attribute directly — appropriate when you already have the `User` object and want its orders. The second uses `.join()` explicitly — appropriate when your starting point is `Order` and you need to filter by something on the related `User`. Both are valid; choose based on which object you start from."

> 🎯 **Instructor Note:** Ask: "Why would you ever need `.join()` if relationship attributes already give you navigation?" Guide learners to: relationship attributes are for navigating from an object you already have. `.join()` is for filtering a query by a condition on a related table when you have not yet loaded the object — it is the SQLAlchemy ORM equivalent of writing the JOIN clause directly, the same JOIN syntax learners already know from SQL, just expressed through method calls.

**Recap of Block 5 before moving on:**

- `db.query(Model)` builds a query; chain `.filter()`, `.order_by()`, `.limit()`; terminate with `.all()`, `.first()`, `.count()`
- `filter()` uses overloaded column operators; `db.get(Model, pk)` is the primary-key shortcut, returning `None` if not found
- Navigate an already-loaded object's relationships directly via attribute access; use `.join()` when filtering a query by a related table's column

---

## Block 6 — Lecture Summary

> 🎯 **Instructor Note:** Deliver as active recall. "What does relationship() actually create versus what mapped_column creates? Where does the foreign key live in a one-to-many relationship? What does secondary= signal, and why is it needed for many-to-many but not one-to-many? What is the difference between filter() and using a relationship attribute directly?"

**SQLAlchemy ORM Introduction**

- ORM maps Python classes to database tables and instances to rows; SQLAlchemy generates parameterized SQL automatically
- Engine holds connection configuration; session manages queries and commits; declarative base registers model classes

**Defining Models and Relationships**

- A model class defines `__tablename__` and columns with `Mapped[type]` and `mapped_column(...)`; `nullable=False` and `unique=True` are the ORM equivalents of `NOT NULL` and `UNIQUE`
- `Base.metadata.create_all(engine)` executes the table creation; the class definition alone does not
- `relationship()` creates a Python-side navigable attribute; it does not by itself define a database column

**One-to-Many and Many-to-Many Relationships**

- One-to-many: the real foreign key column is defined with `mapped_column` and `ForeignKey` on the many side; `relationship()` with `back_populates` makes both directions navigable
- Many-to-many: a junction table is defined as a plain `Table` object with two foreign keys forming a composite primary key, identical in structure to the SQL junction table pattern
- `secondary=junction_table` in `relationship()` tells SQLAlchemy the relationship is mediated through a junction table
- Appending to a relationship list and committing automatically writes the junction table row

**Querying with SQLAlchemy**

- `db.query(Model)` builds a query through chained `.filter()`, `.order_by()`, `.limit()`, executed by a terminator method
- `filter()` uses overloaded column operators; `db.get(Model, pk)` is the primary-key shortcut
- Navigate loaded objects' relationships via attribute access; use `.join()` when filtering by a related table's column directly in a query

**Why All of This Matters Together**

- Defining relationships once in Python and navigating them as attributes removes the repeated manual JOIN-writing and row-tuple handling that raw SQL and a plain database driver require for every relationship access
- The same one-to-many and many-to-many patterns already known from SQL — foreign keys on the many side, junction tables for many-to-many — carry over directly into SQLAlchemy, expressed as Python rather than SQL syntax, which means understanding the SQL foundation directly accelerates understanding the ORM layer built on top of it

---

*End of script.*