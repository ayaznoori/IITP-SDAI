# Lecture Script: Understanding Typical Python Project Structure, pip, Virtual Environments & Practical Utilities
**Duration:** 110 minutes | **Tool:** VS Code + Terminal + venv | **Language:** Python

---

## Session Opening (5 min)

**[Script:]** "You can run Python locally. Today we build like a team: folders, virtual environments, pip, and a real utility that reads and writes text files."

**Problem hook:** You clone a repo, run `python main.py`, and it crashes — missing package, wrong Python, no `data/` folder. Structure and venv fix that.

---

## Why Does This Matter?

🎯 **Instructor Note:** Ask — "Have you ever installed something globally and broken another project?" Connect to venv.

**[Script:]** "Companies do not ship one giant script. They ship projects with layout, locked dependencies, and clear entry points. pip and venv are non-negotiable professional habits."

- **Real-world use:** `requirements.txt`, isolated envs, data folders, CLI utilities
- **Pain if misunderstood:** Dependency conflicts, 'works on my machine', mystery imports, committing `venv/` by accident

---

## What Is the Concept?

### Typical Project Structure

**Mental model:** Kitchen stations — prep (`utils`), pantry (`data`), recipe book (`requirements.txt`), head chef (`main.py`).

```
word_counter/
  main.py
  utils.py
  data/
    input.txt
    output.txt
  requirements.txt
  venv/          # local only — do not commit
```

### Virtual Environments

**Definition:** An isolated Python environment with its own packages.

```bash
python3 -m venv venv
source venv/bin/activate
```

🎯 **Instructor Note:** Show `which python3` before and after activate.

### pip and requirements.txt

**pip** installs third-party packages into the active venv.

```bash
pip install requests
pip freeze > requirements.txt
```

Teammates run:
```bash
pip install -r requirements.txt
```

### Text File I/O in a Utility

**`open()`** with modes:
- `"r"` read
- `"w"` write (overwrites)
- `"a"` append

**Common mistake:** Forgetting to create `data/` before writing.

### Debugging Workflows in VS Code

Set breakpoints in `main.py`, inspect variables, step through file reads.

---

## How Do We Apply It?

### LO 1: Explain a typical Python project structure and the purpose of each folder and file

**Problem:** Whiteboard a utility project before coding.

**Translate logic:** Entry point, helpers, data, dependencies list — label each.

**Predict before building:** Which file do teammates run first? (`main.py`)

**Explain result:** Clear layout reduces onboarding time from hours to minutes.

---

### LO 2: Create and activate virtual environments and install dependencies with pip

**Problem:** Need `requests` without polluting system Python.

**Write code (terminal):**
```bash
python3 -m venv venv
source venv/bin/activate
pip install requests
pip show requests
```

**Predict before running:** Package metadata prints with location inside `venv`.

---

### LO 3: Manage project dependencies using requirements.txt inside an isolated venv

**Problem:** Share exact package versions.

**Write code (terminal):**
```bash
pip freeze > requirements.txt
cat requirements.txt
```

**Demo:** Deactivate, create fresh venv, `pip install -r requirements.txt` — same packages return.

---

### LO 4: Organize a multi-file utility project with modules, data files, and a clear entry point

**Problem:** Word counter utility.

**Write code:**

`utils.py`:
```python
def count_words(text):
    return len(text.split())
```

`main.py`:
```python
from utils import count_words

with open("data/input.txt") as f:
    text = f.read()
count = count_words(text)
with open("data/output.txt", "w") as f:
    f.write(f"Word count: {count}")
```

**Predict before running:** `data/output.txt` contains `Word count: N`.

🎯 **Instructor Note:** Pre-create `data/input.txt` with sample text.

---

### LO 5: Build and run a practical local utility that reads and writes plain-text data within a structured project

**Problem:** Extend utility — if output exists, append a timestamp line.

**Write code:**
```python
from datetime import datetime
from utils import count_words

with open("data/input.txt") as f:
    text = f.read()
line = f"{datetime.now()}: {count_words(text)} words\n"
with open("data/output.txt", "a") as f:
    f.write(line)
print("Done")
```

**Predict:** New line appended each run.

---

## Live Demo Block (20 min)

Build `word_counter/` from empty folder: venv, `utils.py`, `main.py`, `data/`, run, debug one path error live, fix.

---

## Recap (10 min)

🎯 **Instructor Note:** "Why not commit venv?" "What file lists dependencies?"

---

## Lecture Summary

- **Project structure** separates entry point, helpers, data, and config
- **venv** isolates packages per project
- **pip + requirements.txt** make environments reproducible
- **Multi-file utilities** combine modules with file I/O
- **VS Code debugging** speeds up finding path and logic bugs
- **Practical value:** You can now set up a professional Python project others can clone and run

**[Script:]** "Next session we upgrade persistence with JSON and learn to handle errors gracefully — your utility becomes production-minded."
