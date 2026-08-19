# Lecture Script: JSON Basics, File Operations & Exception Handling
**Duration:** 110 minutes | **Tool:** VS Code + Terminal + venv | **Language:** Python

---

## Session Opening (5 min)

**[Script:]** "Your utility saves plain text. Real apps save structured data — user profiles, settings, API payloads. Today: JSON, robust file I/O, and try/except so your program survives bad inputs."

**Problem hook:** A config file is deleted. Without error handling, your script crashes. With `try/except`, it loads defaults and keeps running.

---

## Why Does This Matter?

🎯 **Instructor Note:** Show a sample API JSON response on screen — "This is what fetch returns later in the course."

**[Script:]** "JSON is the lingua franca between Python, JavaScript, and databases. Exception handling separates hobby scripts from tools users trust."

- **Real-world use:** Config files, API bodies, logging, data migration
- **Pain if misunderstood:** `JSONDecodeError`, silent data loss, catching too-broad `Exception`, no validation before access

---

## What Is the Concept?

### JSON Structure and Syntax

**Definition:** JSON is a text format for objects and arrays with strict syntax — double quotes on keys and strings.

| JSON type | Python equivalent |
|-----------|-------------------|
| object `{}` | `dict` |
| array `[]` | `list` |
| string | `str` |
| number | `int` / `float` |
| true/false | `True` / `False` |
| null | `None` |

**Common mistake:** Using single quotes in JSON files — invalid.

### json Module: load and dump

```python
import json

with open("data/user.json") as f:
    user = json.load(f)

with open("data/user.json", "w") as f:
    json.dump(user, f, indent=2)
```

🎯 **Instructor Note:** Contrast `json.loads()` / `json.dumps()` for strings vs files.

### File Operations Recap

- Always use `with open(...)` so files close automatically
- `"r"` vs `"w"` — write mode destroys previous content
- Paths relative to project root when run from `main.py`

### try/except Blocks

**Mental model:** Safety net under a trapeze — catch specific falls, not everything blindly.

```python
try:
    with open("data/config.json") as f:
        config = json.load(f)
except FileNotFoundError:
    config = {"theme": "light"}
```

**Common mistake:** Bare `except:` hiding real bugs — prefer specific exceptions.

### Input Validation

Check keys exist and types match before use:

```python
if "scores" not in user or not isinstance(user["scores"], list):
    raise ValueError("Invalid user data")
```

---

## How Do We Apply It?

### LO 1: Understand JSON structure, syntax rules, and real-world use

**Problem:** Is this valid JSON? `{'id': 1}`

**Translate logic:** Keys must be double-quoted strings → invalid.

**Correct form:**
```json
{"id": 1, "name": "Asha", "tags": ["python", "json"]}
```

**Predict:** Parses in any JSON validator.

---

### LO 2: Map Python dictionaries and lists to JSON-compatible data

**Problem:** Convert in-memory user record to JSON string.

**Write code:**
```python
import json

user = {"name": "Ravi", "scores": [80, 91], "active": True}
text = json.dumps(user, indent=2)
print(text)
```

**Predict before running:** JSON with `true` lowercase, double quotes.

**Explain result:** `dumps` serializes Python objects to JSON text.

---

### LO 3: Read and write persistent JSON using json.load() and json.dump()

**Problem:** Upgrade word-count utility to save stats as JSON.

**Write code:**
```python
import json

stats = {"word_count": 42, "source": "data/input.txt"}
with open("data/stats.json", "w") as f:
    json.dump(stats, f, indent=2)

with open("data/stats.json") as f:
    loaded = json.load(f)
print(loaded["word_count"])
```

**Predict:** `42`

---

### LO 4: Handle runtime errors gracefully with try/except during file and JSON operations

**Problem:** Load config; if missing or corrupt, use defaults.

**Write code:**
```python
import json

default = {"theme": "light", "max_items": 10}
try:
    with open("data/config.json") as f:
        config = json.load(f)
except FileNotFoundError:
    config = default
except json.JSONDecodeError:
    print("Corrupt config — resetting")
    config = default
print(config)
```

**Predict:** Defaults print when file absent.

🎯 **Instructor Note:** Deliberately break JSON file and re-run.

---

### LO 5: Validate inputs and file data before processing

**Problem:** Ensure `scores` is a list of numbers before averaging.

**Write code:**
```python
import json

with open("data/user.json") as f:
    user = json.load(f)

scores = user.get("scores", [])
if not scores or not all(isinstance(s, (int, float)) for s in scores):
    raise ValueError("scores must be a non-empty list of numbers")

avg = sum(scores) / len(scores)
print(round(avg, 2))
```

**Predict:** Valid file → average; invalid → clear `ValueError`.

---

## Live Demo Block (20 min)

Extend prior session's utility: persist settings in `data/config.json`, load on start, handle missing file, validate `input_path` key exists.

---

## Recap (10 min)

🎯 **Instructor Note:** "Name two exceptions we caught today." "Why validate before `user['key']`?"

---

## Lecture Summary

- **JSON** stores structured data in a universal text format
- **Python dicts/lists** map cleanly to JSON objects/arrays
- **`json.load` / `json.dump`** persist data across program runs
- **`try/except`** prevents crashes from missing or corrupt files
- **Validation** catches bad data before it causes silent wrong results
- **Practical value:** Your local utilities now behave like real software — resilient, persistent, and ready for APIs and Git next

**[Script:]** "Next up: Git — version control for the projects you have been building. Your JSON configs and utilities belong in a repo."
