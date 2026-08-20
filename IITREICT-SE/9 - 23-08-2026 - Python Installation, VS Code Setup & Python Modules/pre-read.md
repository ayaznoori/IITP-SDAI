# Pre-Read: Python Installation, VS Code Setup & Python Modules

## Session Mindmap

This diagram shows where this session sits in the course.

```mermaid
flowchart TB
    subgraph foundation ["Foundation"]
        direction TB
        P1["<b>Previous Module</b><br/>Module 1: Programming Foundations<br/><i>[Python · DSA]</i><br/>Data structures · search · sort"]
        CM["<b>Current Module Until Previous Session</b><br/>Module 1: Programming Foundations<br/><i>[Python · Problem Solving]</i><br/>DSA integration · One Compiler workflows"]
    end

    subgraph current ["Current Session"]
        CS["<b>Current Session</b><br/>Python Installation & VS Code Setup<br/><i>Mental shift:</i> from <b>browser coding</b> to <b>local dev</b><br/>Install Python · VS Code · modules · imports"]
    end

    subgraph value ["Course & Real-Life Value"]
        CV["<b>Course Value</b><br/>Gateway to venv, pip, and professional project layout<br/>Matches how teams write Python daily"]
        RL["<b>Real-Life Use</b><br/>Local scripts · Multi-file apps · Shared utility modules · Team repos"]
    end

    subgraph future ["Upcoming Modules"]
        direction TB
        U1["<b>Upcoming Module</b><br/>Module 1: Developer Setup<br/><i>[venv · pip · Project Layout]</i><br/>Dependencies · requirements.txt"]
        U2["<b>Upcoming Module</b><br/>Module 2: Web Fundamentals<br/><i>[HTML · CSS · JavaScript]</i><br/>Frontend pages and styling"]
        U3["<b>Upcoming Module</b><br/>Module 3: FastAPI Backend<br/><i>[REST · Pydantic]</i><br/>API development"]
    end

    P1 ==>|&nbsp;Foundation&nbsp;| CM
    CM ==>|&nbsp;Builds on&nbsp;| CS
    CS ==>|&nbsp;Course Path&nbsp;| CV
    CS ==>|&nbsp;Real-Life Use&nbsp;| RL
    CS ==>|&nbsp;Next Module&nbsp;| U1
    U1 -.-> U2
    U2 -.-> U3

    classDef previous fill:#E8F4FD,stroke:#4A90D9,stroke-width:2px,color:#1a1a1a
    classDef current fill:#FFF3CD,stroke:#E6A817,stroke-width:3px,color:#1a1a1a
    classDef value fill:#D4EDDA,stroke:#28A745,stroke-width:2px,color:#1a1a1a
    classDef future fill:#F3E8FF,stroke:#9B59B6,stroke-width:2px,color:#1a1a1a

    class P1,CM previous
    class CS current
    class CV,RL value
    class U1,U2,U3 future

    linkStyle default stroke-width:3px
```

## 1. What You'll Learn

In this pre-read, you'll discover:

- How to **install Python on your computer** and confirm it works
- How to **set up VS Code** with the integrated terminal for local coding
- How to **run Python scripts locally** instead of only in One Compiler
- How to **split code into modules** and import them across files
- How **import statements and namespaces** connect your project files

---

## 2. Detailed Explanation

### Leaving the Browser Behind

**One Compiler** is great for learning. Real projects run on your machine. Today you move from browser tabs to a **local development workflow** (coding on your own computer with professional tools).

**Analogy:** One Compiler is like a practice kitchen in a classroom. VS Code is your home kitchen where you cook full meals.

### Installing Python

**Python** is the language. You install it once on your laptop. Then you run `.py` files from the terminal.

After install, check it works:

```bash
python3 --version
```

You should see a version number like `Python 3.12.x`.

### VS Code: Your Coding Home Base

**VS Code** (Visual Studio Code) is a free editor where you write code, open folders, and run commands in one place.

Key pieces:
- **Editor** — where you type Python
- **Integrated terminal** — where you run `python3 script.py`
- **Python extension** — helpful hints and run buttons

Open a folder, create `hello.py`, and run it from the terminal inside VS Code.

### Running Scripts Locally

In One Compiler you clicked Run. Locally you type:

```bash
python3 hello.py
```

**Why It Matters:** Local projects can have many files, use packages, and match how teams work.

**Benefits:**
- Work offline after setup
- Organize code in folders like real engineers
- Prepare for virtual environments and pip next session

### Modules: Split Code Across Files

A **module** (a `.py` file treated as a reusable unit) holds related functions and variables.

`greetings.py`:
```python
def say_hello(name):
    return f"Hello, {name}"
```

`main.py`:
```python
import greetings
print(greetings.say_hello("Asha"))
```

### Import Statements

**`import`** brings code from another file into the current file.

Common forms:
- `import math` — built-in module
- `import greetings` — your own file
- `from greetings import say_hello` — import one name

### Namespaces

A **namespace** (the set of names visible in a given scope) decides what names you can use where.

Each module has its own namespace. `greetings.say_hello` means: look in the `greetings` module for `say_hello`.

**Common mistake:** Naming your file `math.py` — it can clash with Python's built-in `math` module.

### Multi-File Projects

A small project might look like:

```
my_project/
  main.py
  utils.py
```

`main.py` imports from `utils.py`. Both files live in the same folder so Python can find them.

---

## 3. Practice Exercises

**Exercise 1 — Local run**
Install Python (or confirm it is installed). Create `hello.py` that prints your name. Run it from the VS Code terminal.

**Exercise 2 — Custom module**
Create `calculator.py` with `add(a, b)`. In `main.py`, import and print `add(3, 5)`.

**Exercise 3 — Trace imports**
In `main.py`, try both `import calculator` and `from calculator import add`. Write one sentence on the difference.
