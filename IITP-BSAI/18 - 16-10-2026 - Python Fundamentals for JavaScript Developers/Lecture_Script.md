# Lecture Script: Python Fundamentals for JavaScript Developers
**Duration:** 110 minutes | **Tools:** VS Code, Terminal, Python 3 | **Language:** Python

---

## Session Timeline Overview

| Block | Time | Focus |
|-------|------|-------|
| Opening | 8 min | Frontend done; servers need Python |
| Why Does This Matter? | 12 min | Jobs, AI SDKs, isolation |
| What Is the Concept? | 22 min | Syntax map, types, venv, pip |
| How Do We Apply It? (LOs) | 53 min | Five LOs live |
| Live lab | 8 min | `hello.py` in venv |
| Recap | 7 min | FastAPI preview only as next class |

---

## Session Opening (8 min)

**[Script:]** "You shipped React. The browser is the front of the shop. The kitchen is the **backend**. That kitchen speaks **Python** in this course. You already know variables, if, loops, and functions. Today we change the spelling, then we lock packages in a **venv** so FastAPI does not fight your system Python."

**Problem hook:** Two students install different package versions globally. Monday the demo works. Friday it does not. Isolation is the fix.

🎯 **Instructor Note:** Check `python3 --version` (or `python --version` on Windows) before anything else. Budget 5 minutes for PATH issues. Do not start FastAPI today.

---

## Why Does This Matter?

🎯 **Instructor Note:** Ask: "Where does `fetch('/api/items')` actually go?" Wait for "a server." Then: "That server is Python from now on."

**[Script:]** "If you skip Python basics, FastAPI looks like magic. If you skip venv, `pip` pollutes the machine and the lab fails at home. Industry treats venv as hygiene, not optional style."

> **In the Real World:** Backend teams at product companies ship Python services. **Netflix**, **Instagram**, and many AI startups expect you to run code in a project environment, not a random global install.

**Pain if misunderstood:**

- `True` vs `true` — silent logic bugs
- Wrong indent — the whole file refuses to run
- `pip` without activate — "module not found" in class

| Frontend habit | Backend habit today |
|----------------|---------------------|
| `npm install` in a project | `pip install` **inside venv** |
| `npm run dev` | `python script.py` |
| `console.log` | `print` |

---

## What Is the Concept?

### Mental model: Same logic, new grammar

**[Script:]** "JavaScript used braces. Python uses **indent**. Miss a space, and Python will not guess."

### Types

- **int**, **float**, **str**, **bool**
- **list** ≈ JS array
- **dict** ≈ JS object (quote the keys)

### Control flow

`if` / `elif` / `else`, `for item in list`, `while`, `def name():`

### venv and pip

**venv** = project-local Python + packages.  
**pip** = installer. Activate first, then install.

🎯 **Instructor Note:** Show a deactivated vs activated prompt. Point at `(.venv)`.

---

## How Do We Apply It?

### LO 1: Map JS ideas to Python syntax

**Problem:** A student knows JS but freezes at Python files.

**Translate logic:** "Declare a flag, a list, and a dict. Print them."

**Write code:**

```python
active = True
tags = ["api", "lab"]
user = {"id": 1, "name": "Asha"}
print(active, tags, user)
```

**Predict before running: What will happen?**

**Predict output:** `True ['api', 'lab'] {'id': 1, 'name': 'Asha'}`

**Explain result:** `True` is capitalized. Dict keys are strings. `print` takes several values.

🎯 **Instructor Note:** Contrast with `true` — NameError. Pause here.

---

### LO 2: Write Python with variables, conditionals, loops, functions

**Problem:** Print pass/fail for a list of marks using one function.

**Translate logic:** Function returns a string. Loop calls it for each mark.

**Write code:**

```python
def status(marks):
    if marks >= 40:
        return "pass"
    else:
        return "fail"

for m in [72, 33]:
    print(m, status(m))
```

**Predict before running: What will happen?**

**Predict output:**

```
72 pass
33 fail
```

**Explain result:** `elif` not needed for two branches. `for` walks the **list**. `return` leaves the function like JS.

🎯 **Instructor Note:** Optional 3-line `while` demo: `n = 0` then `while n < 2`. Keep it tiny.

---

### LO 3: Create and activate a virtual environment

**Problem:** Class packages must not mix with other courses.

**Translate logic:** Create `.venv` in the project folder. Activate so `python` and `pip` point inside it.

**Write code (terminal):**

```bash
python3 -m venv .venv
source .venv/bin/activate
which python
```

**Predict before running: What will happen?**

**Predict output:** Path includes `.venv`. Prompt shows `(.venv)`.

**Explain result:** `python3 -m venv` builds an isolated interpreter. Activate switches your shell to it.

🎯 **Instructor Note:** Windows group: `python -m venv .venv` then `.\.venv\Scripts\Activate.ps1`. Execution policy help one-to-one.

---

### LO 4: Install a package with pip in the venv

**Problem:** Next week needs libraries. Today we prove pip hits the venv.

**Translate logic:** Activate, then `pip install`, then `pip show`.

**Write code:**

```bash
pip install requests
pip show requests
```

**Predict before running: What will happen?**

**Predict output:** Location path under `.venv`.

**Explain result:** pip wrote into the active environment, not system Python.

🎯 **Instructor Note:** Deactivate, `pip show requests` — often missing. Reactivate. The contrast teaches more than a slide.

---

### LO 5: Run a short Python script locally

**Problem:** Proof the toolchain works end to end.

**Translate logic:** File on disk → `python filename.py` with venv on.

**Write code** (`hello.py`):

```python
print("venv is on")
```

```bash
python hello.py
```

**Predict before running: What will happen?**

**Predict output:** `venv is on`

**Explain result:** The active venv's Python executed the file. Same pattern as running Node on a `.js` file.

---

## Live Lab (8 min)

Students: `hello.py` prints their name and a **list** of three skills. Mentor checks venv is active.

> **In the Real World:** First day on a Python team: clone, create venv, pip install, run a script. You practised that loop.

---

## Recap (7 min)

🎯 **Instructor Note:** Cold call: "`else if` in Python?" (`elif`). "Where must you be before pip?" (activated venv).

**[Script:]** "You mapped JS to Python, wrote real control flow, isolated packages, installed with pip, and ran a file. Next class we start FastAPI on this same venv."

---

## Lecture Summary

- **Map JS to Python:** types, `True`/`False`, lists, dicts, indent, `elif`
- **Write Python:** variables, conditionals, loops, functions
- **Create and activate venv:** project-local interpreter
- **pip install inside venv:** packages stay isolated
- **Run a `.py` script locally:** `python file.py`
- **Practical value:** This is the on-ramp to every backend lab in the course
