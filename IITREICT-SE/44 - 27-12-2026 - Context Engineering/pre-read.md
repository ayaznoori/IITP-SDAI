# Pre-Read: Context Engineering

## 1. What You'll Learn

In this pre-read, you'll discover:

- What **context engineering** means in AI-assisted coding
- How to **select the right files and constraints** for a task
- What context you should **avoid** so the model stays accurate
- How to keep AI **grounded in your actual codebase**
- How to apply this to **reduce hallucinations** in a coding exercise

---

## 2. Detailed Explanation

### What Is Context Engineering?

**Context** is everything the model can see: open tabs, pasted files, error logs, your question, and constraints.

**Context engineering** is choosing that bundle on purpose so the AI solves **your** task in **your** repo — not a generic tutorial app.

**One-line definition:** Context engineering is packing the smallest useful truth about the codebase into the prompt.

**Analogy:** A new teammate on day one. If you dump the entire company wiki, they freeze. If you hand them one file, the ticket, and the rule "do not add libraries," they can help.

> **In the Real World:** Tools like **Cursor**, **Copilot Chat**, and **Claude** in IDEs live or die on context. Teams at **GitHub** talk about "grounding." Hallucinated imports are a context failure, not "AI being creative."

### Select the Right Files and Constraints

For a typical FastAPI fix, include:
- The **route file** that fails
- The **helper** it imports
- The **Pydantic models**
- The **exact error** or failing test
- Constraints: Python version vibe, "no new pip packages," "keep max_length validation"

Do **not** start with: "Here is my whole project zip."

**Messy:** 12 unrelated React files + a Python bug.  
**Clear:** `routes/classify.py` + `schemas.py` + traceback.

### Identify Context to Avoid

Avoid packing:
- Huge `venv/` or generated folders
- Secrets and `.env` values
- Unrelated old experiments
- Contradictory README that describes a different architecture
- Entire `node_modules` or lockfiles unless the task is dependency-related
- Five different "maybe useful" files "just in case"

Noise **dilutes** instructions. The model may copy a stale pattern from the wrong file.

### Keep AI Grounded in the Codebase

**Grounded** means: names, paths, and APIs in the reply **exist** in what you showed (or that you will add explicitly).

Grounding habits:
- Paste the **current** function, not what you remember
- Say "Do not invent endpoints. Only use routes in this file."
- After a suggestion, **grep** (search) for the symbol in your repo
- If the model cites `FastAPI.magic()`, reject — that is a hallucination

```text
Task: add a 20-word summary field to TicketOut.
Use only models in schemas.py (pasted).
Do not add a database.
Show the schema change and the one line in the route.
```

### Reduce Hallucinations in a Coding Exercise

A **hallucination** here is invented code: fake modules, fake FastAPI parameters, fake JSON keys your client never sends.

Apply a loop:
1. Pack tight context
2. Generate
3. Check every new identifier against the repo
4. Run the app or a tiny test
5. If wrong, **narrow** context further and name the mistake

**Why It Matters:** Pair programming without context engineering produces confident wrong files. Your Module 4 security work can vanish if the AI "helpfully" removes validation you forgot to include in context.

**Benefits:**
- Fewer fake imports
- Smaller diffs
- Faster reviews
- Model follows **your** contract, not a blog post

### Building Blocks

- Task sentence
- File list (few)
- Constraints
- Error/test
- Verify names exist

---

## 3. Practice Exercises

**Exercise 1 — Define it**  
In two sentences, define context engineering using the "new teammate" analogy.

**Exercise 2 — Select files**  
Bug: `json.loads` fails in `classify_ticket`. List three artifacts you would paste (files or logs). List one you would skip.

**Exercise 3 — Avoid**  
Why is pasting `.env` both a **security** problem and a **context** problem?

**Exercise 4 — Grounding line**  
Add one constraint sentence that forbids inventing new pip packages.

**Exercise 5 — Hallucination check**  
AI adds `from fastapi.magic import Classifier`. Write the two steps you take before accepting.
