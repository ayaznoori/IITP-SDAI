# Pre-Read: LLMOps Foundations

## 1. What You'll Learn

In this pre-read, you'll discover:

- How to **version prompts** and **track changes**
- How to build a **small evaluation set** for prompt quality
- How to run **basic regression checks** when a prompt changes
- How **caching** can cut **latency** and **repeated API cost**
- How to set **simple latency and cost budgets** for an AI feature

---

## 2. Detailed Explanation

### Prompts Are Product Code

When the prompt changes, the product changes. **LLMOps** (LLM operations) is the beginner habit of treating prompts like versioned software: store, test, budget.

**One-line definition:** LLMOps foundations are versioning, tiny evals, regression, cache, and budgets so AI features stay predictable after deploy.

**Analogy:** A restaurant recipe book with dates. If today's dal tastes different, you check which recipe version the kitchen used.

> **In the Real World:** Teams at **OpenAI** customers, **Microsoft**, and AI startups keep prompt files in git. **Intercom** and **Notion** cannot "tweak the prompt on Friday" with no tests — users notice.

### Version Prompts and Track Changes

Keep prompts in files, not only in someone's chat history.

```
prompts/
  classify_v1.txt
  classify_v2.txt
```

Or one file plus git history: commit message `Tighten classifier: add other fallback`.

Track:
- Date / author (git already does)
- Why you changed it
- Which model name you call

**Do not** edit production prompt with no record.

### Small Evaluation Set

An **eval set** is a handful of examples with **expected** properties.

| ticket (short) | expected intent |
|----------------|-----------------|
| want my money back | refund |
| where is the truck | shipping |
| nice cover art | other |

Start with **10–20** rows, not 10,000. Quality over theatre.

You can store them as JSONL or CSV. Run them through the same Python function as production.

### Basic Regression Checks

**Regression** means: a change made something that used to work **fail**.

When you edit the prompt:
1. Run the eval set
2. Compare to last run (how many still match expected intent)
3. Read the **new misses** by hand
4. Only then ship

If score drops, **do not** deploy just because the new prompt feels nicer on one ticket.

### Caching

If the **same** ticket text arrives twice, you may **reuse** the previous JSON instead of calling the API again.

**Analogy:** A librarian keeps yesterday's answer for the same question.

Simple cache: dictionary `hash(text) -> result` in memory. Restart clears it. That is enough to learn the idea.

**Good for:** identical retries, double-clicks.  
**Bad for:** if answers must include "today's time" and you cached yesterday.

Caching reduces **latency** (faster) and **cost** (fewer tokens billed).

> **In the Real World:** **Slack** bots debounce. **CDN** companies cache public pages. LLM cache is the same idea for identical prompts.

### Latency and Cost Budgets

A **budget** is a limit you choose before users complain.

Examples:
- Latency: "p95 classify under 4 seconds" (beginner: "usually under 4s on class wifi")
- Cost: "max 200 completion tokens per ticket" (you already set `max_tokens`)
- Cost: "max N OpenAI calls per user per minute" (idea: do not loop)

Write the budget in the README. When eval or a demo exceeds it, change prompt, cache, or max tokens — not "hope."

**Why It Matters:** Deploy without LLMOps is flying blind. A prompt tweak can break routing and double the bill.

**Benefits:**
- You can explain changes in git
- Fewer silent quality drops
- Double-submit is cheap
- Finance and UX have numbers

### Building Blocks

- Prompt files + git
- Tiny labelled set
- Script that scores matches
- Cache identical inputs
- Written latency/cost limits

---

## 3. Practice Exercises

**Exercise 1 — Version**  
Write a git commit message for changing a prompt to add "JSON only, no markdown."

**Exercise 2 — Eval row**  
Invent two eval tickets: one easy refund, one sarcastic "great, still broken." Guess expected intent for each.

**Exercise 3 — Regression**  
Your match rate went from 9/10 to 6/10 after a prompt edit. Ship or revert? One sentence.

**Exercise 4 — Cache**  
User double-clicks Classify on the same text. How does a cache help latency and cost?

**Exercise 5 — Budget**  
Set one latency budget and one cost budget (in tokens or max_tokens) for the bookstore classifier. Name one knob you would turn if you miss the cost budget.
