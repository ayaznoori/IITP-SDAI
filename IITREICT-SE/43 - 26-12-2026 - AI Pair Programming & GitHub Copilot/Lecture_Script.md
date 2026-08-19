# Lecture Script: AI Pair Programming & GitHub Copilot
**Duration:** 110 minutes | **Tools:** VS Code or compatible editor, GitHub Copilot (or instructor demo if licenses missing), existing FastAPI AI route | **Context:** Module 4 classifier endpoint

---

## Session Timeline Overview

| Block | Time | Focus |
|-------|------|-------|
| Opening & hook | 5 min | Intern vs owner |
| Why Does This Matter? | 10 min | Industry Copilot use |
| What Is the Concept? | 20 min | Pair modes, accept/reject |
| How Do We Apply It? (LOs) | 55 min | Live Copilot lab |
| Standards gate | 15 min | Checklist on a PR-sized diff |
| Recap | 5 min | Context engineering next |

---

## Session Opening (5 min)

**[Script:]** "You can already call OpenAI from FastAPI. Today the model sits **in your editor**. **GitHub Copilot** is a pair programmer: scaffolding, refactors, explanations, tests. You remain the engineer. If you Tab-blind, you will ship bugs that look professional."

**Problem hook:** Show a Copilot suggestion that removes `Field(max_length=2000)`. "Looks cleaner. It also reopens prompt-injection cost attacks."

🎯 **Instructor Note:** If Copilot is unavailable, use chat AI with **paste-the-function-only**. Same LOs.

---

## Why Does This Matter?

🎯 **Instructor Note:** Poll: "Who already Tab-accepts without reading?" Honest hands.

**[Script:]** "**GitHub** built Copilot because boilerplate is expensive. **Stripe** and **Shopify** blogs still stress review. Indian SaaS interviews now ask how you **use** AI — and how you **catch** it lying. Your capstone will be written faster with Copilot. It will only be **hireable** if the code still looks like a human owns it."

**Pain if misunderstood:**
- Fake tests
- Leaked secrets in suggested `.env.example` that is actually a real key
- Refactors that change behaviour
- 400-line dumps you cannot defend in code review

---

## What Is the Concept?

### Pair Programmer Roles

| You ask | Good pair behaviour |
|---------|---------------------|
| Scaffold | New file matching **existing** layout |
| Refactor | Same tests, cleaner structure |
| Explain | Plain language, no extra rewrite |

### Copilot Mechanics

Comments and signatures **steer** grey-ghost text. Chat can draft tests. **You run** tests.

### Debug Mode

Minimal context + error + expected result.

### Reject / Standards

Mental model: **linter + security + style** still win.

**Python vs JS:** Copilot exists in both. This course stays on **Python/FastAPI** files you already have.

---

## How Do We Apply It?

### LO 1: Use AI as a pair for scaffolding, refactoring, explaining

**Problem:** `classify` logic sits inside the route. You want a helper module.

**Translate logic:** Ask: "Extract function `classify_ticket(text: str) -> dict` to `services/classify.py`. Keep Pydantic models in the route. Show only the new file and the new import."

**Walkthrough:** Do it live. Diff should be small.

**Predict before applying:** Will Copilot also invent a database?

**Explain result:** If it invents SQLAlchemy, **reject** that part. Scaffold only what you asked.

> **In the Real World:** **Atlassian** teams often require "one logical change per PR." Pair-AI must respect that.

---

### LO 2: Copilot for suggestions, docstrings, tests

**Demo:** Type:

```python
def urgency_from_text(text: str) -> str:
    """Return 'high' or 'low' from a support ticket string."""
```

Accept or edit the body. Then Copilot Chat: "Write a docstring in Google style and two pytest cases."

**Predict:** Will tests hit the "today" path?

**Explain result:** Keep tests that encode **your** rules. Delete tests for behaviour you did not want.

---

### LO 3: Apply Copilot to debug existing code

**Break a function on purpose** (e.g. wrong key `Intent` vs `intent`). Show the 500.

**Ask Copilot:** include traceback + function + "expected keys intent, urgency."

**Predict:** Does it explain the KeyError?

**Explain result:** Debug pair work is **surgical**. You apply the one-line fix, not a rewrite.

---

### LO 4: Reject unsafe or incorrect suggestions

**Gallery of rejects (slides or live):**
1. Key in source
2. `eval(user_text)`
3. Disable validation
4. `assert True`
5. New npm/pip package not in requirements

Class votes Accept/Reject. Instructor confirms.

**Predict:** Why is `assert True` worse than no test?

**Explain result:** It fakes coverage. QA thinks you tested.

---

### LO 5: Enforce basic code standards with AI code

**Checklist on the extracted helper:**
- snake_case
- type hints
- no unused imports
- docstring
- tests actually import the helper

Run `pytest` (or a tiny assert script if pytest not in project — stay with what they have).

**Predict:** Does passing Copilot's test mean production-ready?

**Explain result:** Tests help. **You** still own behaviour and security from Module 4.

---

## Standards Gate (15 min)

Pairs swap screens. Reviewer uses the reject list. Author must explain every accepted hunk.

---

## Recap (5 min)

**[Script:]** "Next session: **context engineering** — what files you show the AI so it stops inventing APIs you do not have."

---

## Lecture Summary

- Treat AI as a **pair**: scaffold, refactor, explain — you still drive
- **Copilot** speeds suggestions, **docstrings**, and **tests** when you steer it
- Debug with **small** context and the real error
- **Reject** secrets, eval, fake tests, and giant unsolicited rewrites
- Enforce **your** naming, types, and tests on AI code
- **Practical value:** You can go faster without silently lowering the quality bar
