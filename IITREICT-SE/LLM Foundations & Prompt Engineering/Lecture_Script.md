# Lecture Script: LLM Foundations & Prompt Engineering
**Duration:** 110 minutes | **Tools:** Whiteboard, ChatGPT or equivalent playground (no API required) | **Context:** Learners have FastAPI, JSON, and Git from Modules 1–3

---

## Session Timeline Overview

| Block | Time | Focus |
|-------|------|-------|
| Opening & hook | 5 min | Autocomplete vs "knowing" |
| Why Does This Matter? | 12 min | Product features, support bots |
| What Is the Concept? | 28 min | LLM, tokens, window, cutoff, variance |
| How Do We Apply It? (LOs) | 50 min | Zero-shot, few-shot, role, JSON, refine |
| Consistency lab | 10 min | Same task, two prompts, compare |
| Recap & summary | 5 min | LO review + temperature teaser |

---

## Session Opening (5 min)

**[Script:]** "You already ship JSON from FastAPI. Today the 'brain' of many products is not a SQL query. It is a **Large Language Model**. It does not fetch a row. It **guesses the next token**. If you treat it like a database, your feature will hallucinate. If you treat it like a steerable generator, you can build classifiers, summarisers, and helpers that feel reliable."

**Problem hook:** Show two ChatGPT replies to `Write a refund policy in one sentence.` They differ. "Your users will notice. Today we learn why — and how prompts reduce the chaos."

🎯 **Instructor Note:** If playground access is blocked, pair-share and write prompts on paper; run a few live on your machine.

---

## Why Does This Matter?

🎯 **Instructor Note:** Ask — "Name an app that already talks like a person." Collect: ChatGPT, Gemini, Notion AI, Gmail, Instagram captions.

**[Script:]** "Product teams at **Notion**, **Intercom**, and Indian SaaS like **Freshworks** do not ship 'a chatbot' and hope. They write **prompts as product copy**. A vague prompt is a vague feature. A structured prompt is an API contract in English."

**Real-world use:**
- Ticket tagging for **Zendesk**-style queues
- Order-email summaries for **Amazon**-like support
- On-brand rewrites for **Grammarly**-like editors

**Pain if misunderstood:**
- Users get different answers for the same policy question
- JSON parse fails because the model added a joke
- The model invents a return window your legal team never approved
- Long pastes drop your instructions off the **context window**

---

## What Is the Concept?

### Large Language Model

**Definition:** An LLM is a model that maps input text to a probability distribution over the next **token**, then samples tokens until it stops.

**Mental model:** Supercharged autocomplete — not a search index, not a truth oracle.

**Python vs JS:** Same idea in both stacks. You will call it from **Python** next sessions. The prompt language is English (or Hindi), not Python.

### Tokens, Context Window, Knowledge Cutoff

| Term | Plain meaning | Product effect |
|------|----------------|----------------|
| **Token** | Chunk of text the model counts | Cost and length limits |
| **Context window** | Max tokens in + out together | Old instructions can fall off |
| **Knowledge cutoff** | Training data end date | No guaranteed "today's news" |

**[Script:]** "If you paste a 40-page PDF and a one-line question, the question might win or lose depending on order and truncation. Put the **task last**. Keep only the facts you need."

**Common mistakes:**
- Assuming token count equals word count
- Asking about live prices with empty context
- Stuffing the entire GitHub repo into one prompt (later: context engineering)

### Why Outputs Vary

**Definition:** Generation often **samples**. Several next tokens can be "good enough."

**Comparison:** `2 + 2` in Python is stable. `Write a tagline` is a distribution of taglines.

**Common mistakes:**
- Calling the model "broken" because two runs differ
- Using open creative prompts for legal or medical copy

🎯 **Instructor Note:** Pause. Ask: "Should a refund classifier be creative?" Wait for "no."

---

## How Do We Apply It?

### LO 1: Explain LLM, tokens, context windows, knowledge cutoff

**Problem:** A intern says, "The model should know our 2026 Diwali sale. It is famous."

**Translate logic:** Cutoff + no sale text in the prompt = guess or refusal. Put sale rules **in the prompt**.

**Walkthrough (board):**
1. Prompt = instructions + user text
2. Tokens fill the window
3. Reply also uses the window
4. Facts after cutoff must be supplied

**Demo (playground, keep short):**

```
Using only this fact: "Sale ends 20 Dec 2026."
Answer: When does the sale end?
If the fact is missing, say "unknown".
```

**Predict before running:** What will happen if we delete the fact sentence?

**Explain result:** The model should say unknown (if the rule is strong). Without the rule it may invent a date.

---

### LO 2: Explain why LLM outputs can vary for the same prompt

**Problem:** QA files a bug: "Bot gave two different greetings."

**Translate logic:** Open-ended wording invites sampling. Tight format reduces spread.

**Write (run twice):**

```
Greet a customer named Asha in a fun way.
```

Then:

```
Reply with exactly: "Hello Asha, how can I help today?"
```

**Predict before running:** Which prompt stays the same across two runs?

**Explain result:** Constrained copy is more repeatable. Fun greetings wander.

> **In the Real World:** **PhonePe** in-app help cannot wander on KYC steps. Marketing slogans for **Myntra** can wander.

---

### LO 3: Write zero-shot and few-shot prompts

**Problem:** Classify support mail as `refund`, `shipping`, or `other`.

**Zero-shot (write on board, then run):**

```
Classify the email. Reply with one word: refund, shipping, or other.
Email: "Package shows delivered but I do not have it."
```

**Predict:** refund, shipping, or other?

**Few-shot:**

```
Classify. Reply with one word: refund, shipping, or other.

Email: "I want my money back for order 12."
Label: refund

Email: "Where is truck for order 12?"
Label: shipping

Email: "Package shows delivered but I do not have it."
Label:
```

**Predict before running:** Did examples pull "delivered but missing" toward shipping?

**Explain result:** Few-shot teaches the **decision boundary**, not just the label list.

🎯 **Instructor Note:** Do not introduce fine-tuning. Examples in the prompt are enough.

---

### LO 4: Role prompting and structured output

**Problem:** Python must parse the answer. Free prose breaks `json.loads`.

**Role + schema:**

```
You are a careful ticket router for an online electronics store.
Use only the email. Do not invent order IDs.
Return JSON only:
{"intent":"refund"|"shipping"|"other","urgency":"low"|"high"}
Email: "Laptop arrived dead. I have a flight in 6 hours."
```

**Predict:** urgency high or low? intent?

**Explain result:** Role sets caution. Schema makes the result machine-readable. Urgency comes from "flight in 6 hours."

**Second mini demo — JSON gone wrong:** Ask once without "JSON only." Show markdown fences. "That is why we say no markdown."

---

### LO 5: Refine prompts for consistency

**Problem:** First classifier sometimes returns `Refund!!` or a paragraph.

**Refine checklist (live edit):**
1. Add allowed labels
2. Add "lowercase, one word" or JSON
3. Add fallback `other`
4. Add one few-shot for sarcasm: "amazing /s still broken"
5. Re-run the same three emails twice

**Predict:** Does sarcasm still map to `other` or `refund` after the extra example?

**Explain result:** Consistency is **iteration**, not one clever sentence.

---

## Consistency Lab (10 min)

Pairs: pick a **Swiggy-style** "late delivery" email. Write (a) zero-shot apology in 15 words, (b) few-shot using two brand-tone examples. Run both twice. Tally which drifted more.

🎯 **Instructor Note:** Collect one "drift" screenshot. Celebrate the tighter prompt.

---

## Recap (5 min)

**[Script:]** "Next session we add **knobs**: temperature, max tokens, stop sequences, penalties. Prompts are the recipe. Parameters are the oven settings."

---

## Lecture Summary

- An **LLM** predicts tokens; it is not a database of truths
- **Tokens**, **context windows**, and **knowledge cutoff** cap what the model can use
- **Outputs vary** because generation samples among likely continuations
- **Zero-shot** states the task; **few-shot** shows the pattern
- **Role + structured output** make replies safer to parse and on-brand
- **Refining** prompts (rules, fallbacks, examples) is how products get consistent
- **Practical value:** Every AI feature you ship starts as a prompt you can explain and test
