# Lecture Script: Intro to AI & Prompt Engineering
**Duration:** 110 minutes | **Tools:** Chat UI (Claude or ChatGPT), Whiteboard | **Tone:** Builder, not mystic

---

## Session Timeline Overview

| Block | Time | Focus |
|-------|------|-------|
| Opening | 8 min | Autocomplete, not an oracle |
| Why Does This Matter? | 12 min | Product prompts, Cursor quality |
| What Is the Concept? | 25 min | LLM, tokens, messages |
| How Do We Apply It? (LOs) | 52 min | System/user, shots, rewrite |
| Live lab | 8 min | Improve one weak prompt |
| Recap | 5 min | API next |

---

## Session Opening (8 min)

**[Script:]** "You have chatted with models. Today we put builder names on the machine. An **LLM** predicts **tokens**. It has a **context window**. You steer it with a **system prompt** and a **user message**. You will try **zero-shot** and **few-shot**, then **repair a weak prompt**. No training neural nets. No embedding libraries. Language as an interface."

**Real-world hook:** Show **Notion AI** “help me write.” "Someone wrote the system instructions behind that button."

🎯 **Instructor Note:** Students may use any major chat UI. Ban pasting private keys. Stay inside LOs.

---

## Why Does This Matter?

🎯 **Instructor Note:** Run a vague prompt live (“fix this”) vs a sharp one. Compare outputs.

**[Script:]** "At **Duolingo** and **Grammarly**, prompts are product. At **Masai**-style teaching too. If you cannot write a system message, you will dump secrets into the frontend next week. If you ignore the context window, you will paste a whole repo and wonder why the model forgot the bug."

**Pain if misunderstood:**
- Believing the model “looked up” your private DB
- No output format — unusable in apps
- Few-shot examples that contradict the rules

> **In the Real World:** **OpenAI** and **Anthropic** docs start with messages and roles. That is the industry contract.

---

## What Is the Concept?

**LLM:** next-token predictor with useful behaviour from training.

**Token:** text chunk.

**Context window:** capacity of one request.

**Zero-shot / few-shot:** no examples vs a few input/output pairs.

**Python vs JS:** irrelevant — prompts are text. APIs come next session.

**Common mistake:** “The model knows my Neon data.” It does not, unless you paste it (and you should not paste secrets).

---

## How Do We Apply It?

### LO 1: Explain what an LLM is at a builder level

**Problem:** Magical thinking.

**Translate logic:** Input tokens in, likely tokens out, instructions bias the path.

**Predict:** Can it know Tuesday’s internal sales if not in the prompt? (No.)

---

### LO 2: Describe tokens and context window simply

**Whiteboard backpack.** Short vs huge paste.

**Predict:** If the window is full, what gets dropped first in a naive chat? (Oldest turns — conceptual, keep simple.)

**[Script:]** "You do not count tokens by hand today. You write shorter, structured prompts."

---

### LO 3: Write a system prompt and a user message

```text
SYSTEM: You are a note title helper. Return only JSON: {"title": string}.
USER: need to call dentist friday
```

**Predict before running:** Will extra markdown appear if we say “only JSON”? (Often still leak — we tighten next.)

**Explain result:** Roles separate standing law from this request.

---

### LO 4: Apply zero-shot and few-shot on a sample task

**Task:** Label feedback `bug` or `feature`.

**Zero-shot:** define labels only.

**Few-shot:** two examples each.

**Predict before running:** Which is more stable on format? (Few-shot, usually.)

> **In the Real World:** **Customer support** classifiers at **Freshdesk**-like products start as few-shot before any fine-tune.

---

### LO 5: Improve a weak prompt into a clearer one

**Weak:** "Summarise."

**Clinic:** add audience, length, must-keep facts, forbidden extras.

**Live rewrite on projector.** Students vote which version they would ship.

---

## Live Lab (8 min)

Each student: one weak prompt from their project idea → improved system + user. Share in pairs.

---

## Recap (5 min)

**[Script:]** "Next session the prompt lives on the **server**. React will only send the user task. Keys stay off the frontend."

---

## Lecture Summary

- An **LLM** is a next-token engine you steer with text, not a source of guaranteed truth
- **Tokens** fill a limited **context window**, so prompts must stay tight
- **System** vs **user** messages separate rules from the request
- **Zero-shot** is instructions only; **few-shot** adds examples for picky formats
- A **clearer prompt** states audience, constraints, and output shape
- **Practical value:** You can design AI behaviour before you write API code
