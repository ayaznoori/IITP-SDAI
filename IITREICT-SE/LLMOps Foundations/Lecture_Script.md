# Lecture Script: LLMOps Foundations
**Duration:** 110 minutes | **Tools:** Git, Python, FastAPI classify function, simple JSON eval file | **Context:** Deployed/containerised app from prior session

---

## Session Timeline Overview

| Block | Time | Focus |
|-------|------|-------|
| Opening & hook | 5 min | Friday prompt tweak |
| Why Does This Matter? | 10 min | Quality + bills |
| What Is the Concept? | 22 min | Version, eval, cache, budgets |
| How Do We Apply It? (LOs) | 55 min | Live eval script |
| Budget design | 13 min | Class sets numbers |
| Recap | 5 min | Testing next |

---

## Session Opening (5 min)

**[Script:]** "You can Docker the API. You can still break it with a **prompt edit** nobody recorded. **LLMOps foundations**: version prompts, a **small eval set**, **regression** when prompts change, **cache** for repeats, and **latency/cost budgets**."

**Problem hook:** Two git diffs — code unchanged, `classify_v2.txt` changed one sentence. "This is a product release."

---

## Why Does This Matter?

🎯 **Instructor Note:** Ask who has no idea which prompt is in production.

**[Script:]** "**OpenAI** usage dashboards, **Datadog** traces at bigger companies, **Freshworks** support AI — someone owns quality. Your version is a CSV and git. That is enough to be professional at this stage. Finance will ask why tokens doubled. 'The model was moody' is not an answer. **Budgets** are."

**Pain if misunderstood:**
- Prompt only in Slack
- Ship a 'better' prompt that fails old tickets
- Double-clicks drain quota
- No latency target → endless spinner

---

## What Is the Concept?

### Prompt Versioning

Files + git messages. Optional `PROMPT_VERSION=v2` env for logs.

### Eval Set

Tiny labelled truth.

### Regression

Re-run eval; compare counts.

### Caching

Map identical input → stored output.

### Budgets

Written limits; knobs: max_tokens, cache, shorter prompt.

---

## How Do We Apply It?

### LO 1: Version prompts and track changes

**Move** system text from a string in `main.py` to `prompts/classify_v1.txt`. Load in Python. Commit.

**Predict:** If two servers load different files, what happens?

**Explain result:** Version drift. Pin filename or env `PROMPT_VERSION`.

> **In the Real World:** Feature flags at **LaunchDarkly**-using teams sometimes flag prompt versions. You use git.

---

### LO 2: Small evaluation set

**Write 8–12 JSON rows live** covering refund, shipping, other, plus one injection-like string (expect still a valid label).

```python
evals = [
    {"text": "I want a refund", "intent": "refund"},
]
```

**Predict:** Should eval texts be huge novels?

**Explain result:** No. Small, labelled, representative.

---

### LO 3: Basic regression checks when prompts change

**Script:** loop evals, call classify (or a mocked function if keys are scarce — prefer real 5 rows). Print `pass_count/total`.

Change prompt to be overly creative. Re-run. Score drops.

**Predict before second run:** Direction of the score?

**Explain result:** Regression caught the 'nicer' prompt. Revert.

🎯 **Instructor Note:** If API cost is a concern, freeze responses for the demo after one collection.

---

### LO 4: Caching to reduce latency/cost

**In-memory dict** in the route (process-local). Log `cache_hit` vs `miss`.

Double-submit same ticket in `/docs`.

**Predict:** Second call hits OpenAI?

**Explain result:** Hit → faster, cheaper. Mention: many workers = many caches. Fine for foundation.

---

### LO 5: Simple latency and cost budgets

**Board:**
- Latency: show `time.perf_counter()` around the call; target e.g. 5s class network
- Cost: `max_tokens=64`; log `usage` if present

If over budget: shorten prompt, lower max tokens, rely on cache — **inside these LOs**.

**Predict:** Does a 4000-token max help a JSON label?

**Explain result:** It hurts the cost budget for no quality gain.

---

## Budget Design (13 min)

Pairs write three lines in README: prompt file path, eval command, latency/cost targets.

---

## Recap (5 min)

**[Script:]** "Next: **AI testing and code review** — evals were for prompts. Tests and PR review are for **code**, still with a human owner."

---

## Lecture Summary

- **Version prompts** in git like code
- A **small eval set** makes quality visible
- **Regression** runs when prompts change
- **Cache** identical requests to save time and money
- **Latency and cost budgets** turn vibes into numbers
- **Practical value:** You can change a prompt without flying blind in production
