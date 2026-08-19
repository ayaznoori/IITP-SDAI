# Lecture Script: Python Installation, VS Code Setup & Python Modules
**Duration:** 110 minutes | **Tool:** VS Code + Terminal | **Language:** Python

---

## Session Opening (5 min)

**[Script:]** "You have been coding in One Compiler. Today you install Python on your machine, open VS Code, and run scripts locally. This is the shift from classroom kitchen to home kitchen."

**Problem hook:** A teammate sends you a folder with `main.py` and `utils.py`. You cannot run it in a browser tab. You need local Python and imports.

---

## Why Does This Matter?

🎯 **Instructor Note:** Ask — "What breaks when you only know browser-based coding?" (no files, no modules, no pip later)

**[Script:]** "Professional Python lives on your laptop. Teams share folders, not links. Modules keep code organized. If you skip local setup, every future session — venv, pip, JSON files — stalls."

- **Real-world use:** Local scripts, multi-file projects, shared utility modules, team repos
- **Pain if misunderstood:** Wrong Python version, PATH errors, import failures, naming files `math.py` and shadowing built-ins

---

## What Is the Concept?

### Installing Python

**Definition:** Python is the interpreter that runs `.py` files on your operating system.

**Mental model:** The engine under the hood — VS Code is the dashboard; Python is what actually moves.

**Check install:**
```bash
python3 --version
```

🎯 **Instructor Note:** Windows users may use `python` instead of `python3` — note platform differences without deep dive.

### VS Code + Integrated Terminal

**Definition:** VS Code is an editor where you write code, open folders, and run terminal commands in one window.

| Piece | Role |
|-------|------|
| Editor | Write `.py` files |
| Explorer | See project files |
| Terminal | Run `python3 script.py` |
| Python extension | Syntax help and run shortcuts |

**Common mistake:** Saving files outside the opened folder — imports fail because Python cannot find sibling modules.

### Running Scripts Locally

**Browser (One Compiler):** Click Run.

**Local:** Type in terminal:
```bash
python3 hello.py
```

### Modules and Imports

**Definition:** A module is a `.py` file whose functions and variables you reuse via `import`.

**Mental model:** Toolbox drawers — each file is a drawer; `import` opens the drawer you need.

**Common forms:**
- `import math`
- `import greetings`
- `from greetings import say_hello`

### Namespaces

**Definition:** A namespace is the set of names visible in a given scope.

**[Script:]** "`greetings.say_hello` means: look inside the `greetings` module for that name."

**Common mistake:** File named `random.py` shadows Python's `random` module.

---

## How Do We Apply It?

### LO 1: Install Python locally and configure VS Code with the integrated terminal

**Problem:** Confirm Python works and VS Code can run commands.

**Translate logic:** Install → verify version → open VS Code → open terminal → run version command again.

**Write code (terminal only):**
```bash
python3 --version
```

**Predict before running:** A version string like `Python 3.12.x`.

**Explain result:** If this works, the interpreter is on PATH and ready for scripts.

---

### LO 2: Run Python scripts locally and transition from One Compiler to a local project

**Problem:** Print your name from a file on disk.

**Translate logic:** Create folder → create `hello.py` → run from terminal inside that folder.

**Write code:**
```python
# hello.py
print("Hello from local Python!")
```

**Run:**
```bash
python3 hello.py
```

**Predict before running:** `Hello from local Python!`

**Explain result:** The terminal passes the file to Python; output appears in the same terminal panel.

---

### LO 3: Organize code into modules and import them across project files

**Problem:** Share a `greet` function between files.

**Write code:**

`greetings.py`:
```python
def greet(name):
    return f"Hello, {name}"
```

`main.py`:
```python
import greetings
print(greetings.greet("Asha"))
```

**Predict before running:** `Hello, Asha`

🎯 **Instructor Note:** Both files must sit in the same folder opened in VS Code.

---

### LO 4: Explain namespaces and how import statements resolve module access

**Problem:** Show two import styles for the same function.

**Write code:**
```python
import greetings
from greetings import greet
print(greetings.greet("Ravi"))
print(greet("Ravi"))
```

**Predict:** Both lines print `Hello, Ravi`.

**Explain result:** `import greetings` keeps the module namespace; `from ... import` brings one name into the current namespace.

---

### LO 5: Create and use custom Python modules across a small multi-file script

**Problem:** Build a two-file calculator mini-project.

**Write code:**

`calculator.py`:
```python
def add(a, b):
    return a + b
```

`main.py`:
```python
from calculator import add
print(add(10, 5))
```

**Predict:** `15`

---

## Live Demo Block (15 min)

Full walkthrough: Create `my_project/` with `main.py` and `utils.py`, run from VS Code terminal, intentionally break import by wrong filename, fix together.

---

## Recap (10 min)

🎯 **Instructor Note:** "What command runs a Python file?" "What is a module?" Quick chorus.

---

## Lecture Summary

- **Local Python + VS Code** replaces browser-only workflows for real projects
- **Integrated terminal** is how you execute scripts like engineers do
- **Modules** split code into reusable files
- **`import` and namespaces** control which names are visible where
- **Multi-file scripts** are the foundation for venv, pip, and structured projects next session
- **Practical value:** You can now work offline, organize code in folders, and match team development habits

**[Script:]** "Next session we add project structure, virtual environments, and pip — the professional dependency layer on top of today's setup."
