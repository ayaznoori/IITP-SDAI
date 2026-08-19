# Lecture Script: Context Engineering
**Duration:** 110 minutes | **Tools:** Editor AI chat, repo from Module 4–5 | **Context:** Copilot pair habits from prior session

---

## Session Timeline Overview

| Block | Time | Focus |
|-------|------|-------|
| Opening & hook | 5 min | Whole-repo paste fail |
| Why Does This Matter? | 10 min | Hallucinated APIs |
| What Is the Concept? | 22 min | Pack, avoid, ground |
| How Do We Apply It? (LOs) | 55 min | Live coding exercise |
| Hallucination hunt | 13 min | Spot fake symbols |
| Recap | 5 min | Specs next |

---

## Session Opening (5 min)

**[Script:]** "Yesterday you learned to reject bad Copilot. Today we prevent bad Copilot by **feeding it better lunch**. **Context engineering** is choosing files, errors, and constraints so the model stays **grounded** in this repo."

**Problem hook:** Ask chat: "Add caching to my FastAPI app" with **no files**. Watch it invent Redis. "We do not have Redis. That was a hallucination of a **missing context**."

---

## Why Does This Matter?

🎯 **Instructor Note:** Ask for a time AI invented a function that did not exist. Collect stories.

**[Script:]** "Product teams using **Cursor** or **Copilot Workspace**-style flows pack tickets with **linked files**. **Uber** and **Meta** scale is different, but the junior skill is the same: **small truth beats big noise**. Hallucinated routes break **Swagger** contracts your frontend trusts."

**Pain if misunderstood:**
- Fake imports, pip install rabbit holes
- Security checks dropped because they were not in the paste
- Model follows README from 2023, not the code from last week

---

## What Is the Concept?

### Definition

**Context engineering** = design of the prompt bundle (files + rules + task).

### Select vs Avoid

**Include:** failing unit, caller, schema, error.  
**Exclude:** venv, secrets, unrelated apps, contradictory docs.

### Grounding

**Mental model:** The model may only "see" what you showed. Everything else is a guess.

**Common mistake:** "The AI has my whole GitHub." In this session, assume it only has what you attach.

---

## How Do We Apply It?

### LO 1: Explain what context engineering means

**Problem:** Intern says "just @codebase everything."

**Translate logic:** More tokens ≠ more truth. Windows fill (remember Module 4). Old files **crowd out** the bug.

**Board definition:** smallest useful truth + constraints.

**Predict:** If we add 40 files, do instructions at the top still matter as much?

**Explain result:** They can fall off or get mixed. Pack on purpose.

---

### LO 2: Select the right files and constraints

**Exercise setup:** Add `summary: str` to `TicketOut` and fill it with a 12-word model line **or** a placeholder if you are offline — stay inside existing OpenAI helper.

**Select live:** `schemas.py`, classify route, client helper, **one** test.

**Constraints to type:**

```
No new dependencies.
Keep max_length on TicketIn.
Do not remove injection DATA fences.
```

**Predict:** Which file, if omitted, causes hallucinated field names?

**Explain result:** Without schema, the model invents `TicketResponse`.

---

### LO 3: Identify context to avoid

**Show a bad pack:** `.env`, `venv/pyvenv.cfg`, a random `scratch.js`, an old README describing Flask.

**Predict:** Which item causes a Flask rewrite?

**Explain result:** Contradictory docs are poison. Secrets are never context.

> **In the Real World:** Incident reports often start with a key pasted into a chatbot.

---

### LO 4: Keep AI grounded in the codebase

**After generation:** instructor greps every new symbol.

**Demo question:** "List functions you used and the file they came from." If the model cannot point to the paste, treat as suspect.

**Predict:** Can the model honestly cite a file you did not attach?

**Explain result:** Grounding is **evidence**, not vibe.

---

### LO 5: Apply to reduce hallucinations in a coding exercise

**Live task:** Students pack context, generate the `summary` field change, then:
1. Search project for invented names
2. Run `/docs` or a pytest
3. If hallucination, **shrink** paste and add "You invented X. Remove it."

**Predict before running tests:** Will Swagger show `summary`?

**Explain result:** The loop **pack → generate → verify** is the skill. Not one perfect prompt.

🎯 **Instructor Note:** Keep the exercise tiny so everyone finishes verify.

---

## Hallucination Hunt (13 min)

Pairs swap an AI diff. Highlight invented APIs in red. Count. Lowest count wins.

---

## Recap (5 min)

**[Script:]** "Next: you will use AI to draft **PRDs and UI flows** — still with human refine. Bad context there invents features. Same muscle."

---

## Lecture Summary

- **Context engineering** is packing the smallest useful truth for the model
- **Select** the failing files, tests, and explicit constraints
- **Avoid** secrets, venv, and contradictory extra files
- **Ground** replies in names that exist in the repo
- **Verify** to cut hallucinations in a real coding exercise
- **Practical value:** AI help stays about *this* FastAPI app, not a blog's app
