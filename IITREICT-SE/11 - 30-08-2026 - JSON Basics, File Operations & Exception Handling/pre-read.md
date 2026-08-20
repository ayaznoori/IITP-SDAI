# Pre-Read: JSON Basics, File Operations & Exception Handling

## Session Mindmap

This diagram shows where this session sits in the course.

```mermaid
flowchart TB
    subgraph foundation ["Foundation"]
        direction TB
        P1["<b>Previous Module</b><br/>Module 1: Programming Foundations<br/><i>[Python · DSA]</i><br/>Data structures · algorithms"]
        CM["<b>Current Module Until Previous Session</b><br/>Module 1: Developer Setup<br/><i>[venv · pip · Project Layout]</i><br/>Structured projects · text-file utilities"]
    end

    subgraph current ["Current Session"]
        CS["<b>Current Session</b><br/>JSON, Files & Exceptions<br/><i>Mental shift:</i> from <b>fragile scripts</b> to <b>resilient tools</b><br/>JSON persistence · try/except · validation"]
    end

    subgraph value ["Course & Real-Life Value"]
        CV["<b>Course Value</b><br/>Data format shared with web APIs and FastAPI later<br/>Defensive coding before Git collaboration"]
        RL["<b>Real-Life Use</b><br/>Config files · API payloads · safe file pipelines · user-proof utilities"]
    end

    subgraph future ["Upcoming Modules"]
        direction TB
        U1["<b>Upcoming Module</b><br/>Module 1: Developer Setup<br/><i>[Git · GitHub]</i><br/>Version control · remote repos"]
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

- What **JSON** is and why apps use it to store structured data
- How **Python dictionaries and lists** map to JSON objects and arrays
- How to **read and write JSON files** with the `json` module
- How to use **`try/except`** to handle errors without crashing your program
- How to **validate inputs and file data** before processing

---

## 2. Detailed Explanation

### Why JSON?

**JSON** (JavaScript Object Notation — a text format for structured data) is how APIs, config files, and mobile apps exchange information.

**Analogy:** JSON is a universal packing box. Python dicts, JavaScript objects, and databases all know how to pack and unpack it.

### JSON Structure

JSON uses:
- **Objects** — `{ "key": "value" }` (like Python dicts)
- **Arrays** — `[1, 2, 3]` (like Python lists)
- **Strings, numbers, booleans, null**

```json
{
  "name": "Asha",
  "scores": [88, 92, 79],
  "active": true
}
```

### Python ↔ JSON Mapping

| Python | JSON |
|--------|------|
| `dict` | object |
| `list` | array |
| `str` | string |
| `int` / `float` | number |
| `True` / `False` | true / false |
| `None` | null |

### Reading and Writing Files

You already used `open()` for plain text. JSON adds `json.load()` and `json.dump()`:

```python
import json

with open("data/user.json") as f:
    user = json.load(f)

user["scores"].append(95)

with open("data/user.json", "w") as f:
    json.dump(user, f, indent=2)
```

### Exception Handling

Programs crash when files are missing or JSON is malformed. **`try/except`** catches errors so you can respond gracefully.

```python
try:
    with open("data/missing.json") as f:
        data = json.load(f)
except FileNotFoundError:
    print("File not found — using defaults")
    data = {}
```

**Why It Matters:** Real users mistype paths, delete files, or send bad data. Defensive code keeps utilities running.

**Benefits:**
- Persist structured data between runs
- Share data with web frontends and APIs later
- Build reliable tools that fail with helpful messages, not stack traces

### Input Validation

Before processing, check:
- Does the file exist?
- Is required data present (`"name"` in dict)?
- Are types correct (is `scores` a list)?

Small checks prevent confusing bugs downstream.

---

## 3. Practice Exercises

**Exercise 1 — JSON spot check**
Which is valid JSON: `{'name': 'Asha'}` or `{"name": "Asha"}`? Write why.

**Exercise 2 — Dict to JSON**
Create a Python dict with your name and three hobbies. Use `json.dumps()` to print it as a JSON string.

**Exercise 3 — Safe read**
Write pseudocode (plain English steps) for: try to load `data/config.json`; if file missing, create an empty dict and save it.
