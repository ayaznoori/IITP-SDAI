# Lecture Script: Controllability & Tradeoffs
**Duration:** 110 minutes | **Tools:** LLM playground with parameter sliders | **Context:** Prompts from prior session (classifier, summariser, slogans)

---

## Session Timeline Overview

| Block | Time | Focus |
|-------|------|-------|
| Opening & hook | 5 min | Same prompt, different oven |
| Why Does This Matter? | 12 min | Cost, QA, brand |
| What Is the Concept? | 25 min | Temp, max tokens, stop, penalties |
| How Do We Apply It? (LOs) | 50 min | Tune a product bundle |
| Tradeoff debate | 13 min | Two teams, one feature |
| Recap | 5 min | API call teaser |

---

## Session Opening (5 min)

**[Script:]** "Yesterday the prompt was the recipe. Today we turn the oven. **Temperature**, **max tokens**, **stop sequences**, and **penalties** are how **OpenAI-style APIs** let products stay cheap, short, and on-brand — or wild on purpose."

**Problem hook:** Run `Give 5 product names for a mango drink` at temperature 0 and 1.2. Show the board. "Neither is 'wrong.' One is a **product choice**."

---

## Why Does This Matter?

🎯 **Instructor Note:** Ask — "If each extra 100 tokens costs money, who feels the pain: user or finance?" Both. Latency too.

**[Script:]** "**Gmail** smart replies stay tiny. **Notion** drafts can be long. **Razorpay** support cannot invent fee numbers. Your job is to pick knobs that match the **use case**, then **compare tradeoffs** out loud with a PM."

**Real-world use:**
- Cap completions so mobile SMS features stay cheap
- Low temperature for KYC/help text at **PhonePe**-like products
- High temperature for **Spotify** Wrapped-style playful copy (tone only — still review facts)

**Pain if misunderstood:**
- Truncated JSON → app crash
- High temp on policies → made-up rules
- No stop in a dialogue template → model plays both sides
- Heavy presence penalty → required field names vanish

---

## What Is the Concept?

### Temperature

**Definition:** A setting that flattens or sharpens next-token probabilities.

**Mental model:** Low = careful student. High = improv actor.

**When it matters:** Open tasks and "pick one of N labels" both feel temperature; labels need low.

**Common mistake:** "Temperature 0 means always identical." Often more stable, **not** a mathematical guarantee for all systems.

### Max Tokens

**Definition:** Upper bound on generated tokens (output budget).

**Comparison:** Like `LIMIT` in SQL for **how much** comes back — not a quality score.

**Common mistake:** Huge default "just in case" → surprise bills and slow UX.

### Stop Sequences

**Definition:** Strings that immediately end generation.

**Mental model:** A period you put in the machine, not only in the sentence.

**Common mistake:** Stopping on `"` inside JSON and cutting the string early.

### Frequency vs Presence Penalty

| Penalty | Targets | Typical use |
|---------|---------|-------------|
| **Frequency** | Tokens used many times | "very very" loops |
| **Presence** | Tokens used at least once | Push new ideas |

**Common mistake:** High penalties on structured output.

---

## How Do We Apply It?

### LO 1: When temperature matters

**Problem:** Marketing wants 10 taglines. Legal wants one disclaimer sentence.

**Translate logic:** Taglines = higher temp. Disclaimer = near 0.

**Demo (same prompt, two temps):**

```
Write one sentence: 7-day returns on unopened items only.
```

Run at 0 and 1.0.

**Predict before running:** Which run stays closer to the legal words?

**Explain result:** High temp paraphrases; paraphrases can change meaning ("7-day" → "about a week").

---

### LO 2: Apply max tokens for length and cost

**Problem:** Feature must return a **one-line** order status for an app banner.

**Write settings on board:** `max_tokens = 24` vs `256`.

**Demo prompt:**

```
Status line only. No greeting.
Order 99 is out for delivery.
```

**Predict:** What happens if the model starts a story under max 24?

**Explain result:** Cutoff risk. Combine small max tokens **with** "one line" in the prompt. Cost: 24 vs 256 is an obvious budget win.

> **In the Real World:** **Swiggy** push notifications have character limits. Your max tokens should respect the channel.

---

### LO 3: Use stop sequences

**Problem:** Fill-in-the-blank email:

```
Subject: Update on your order
Body:
```

**Demo:** Set stop to `\nSubject:` or `Customer:` depending on template. Show one run without stop where the model adds a fake second email.

**Predict before running:** Without stop, will the model invent another heading?

**Explain result:** Stops keep the model inside the blank.

🎯 **Instructor Note:** If the playground stop box is hidden, simulate by cutting text at the first blank line and explain the API field.

---

### LO 4: Frequency and presence penalties

**Problem:** Brainstorm names; output is `MangoMan MangoMan MangoMan`.

**Demo:** Same prompt, `frequency_penalty` 0 vs 0.8 (use whatever scale the playground shows; explain direction).

**Predict:** Will names diversify or become weird spellings?

**Explain result:** Mild frequency penalty helps. Too much presence penalty may drop the word `mango` entirely — bad for the brief.

---

### LO 5: Tune for a product use case and compare tradeoffs

**Product brief (live):** "Helpdesk helper for **an online bookstore** (think **Amazon** books). Output JSON `{"tone":"calm","next_step":"..."}` plus a 20-word user-facing line."

**Team A bundle:** temp 0, max 80, no penalties, stop `\n\n`  
**Team B bundle:** temp 0.9, max 300, presence 0.6

Run the **same** three tickets on both. Score:
- Parse success
- Policy safety (no invented refund days)
- Cost proxy (output length)
- Helpfulness

**Predict:** Which team wins parse + safety? Which wins colourful copy?

**Explain result:** Tradeoffs are **visible**. You do not pick "best AI." You pick **best bundle for the job**.

---

## Tradeoff Debate (13 min)

Two groups argue for **only one** bundle for the bookstore bot. Instructor is "PM." Force a decision: ship A or B, and name **one** follow-up test.

---

## Recap (5 min)

**[Script:]** "Next session those knobs become Python arguments: `temperature`, `max_tokens`, `stop`. Bring your winning bundle."

---

## Lecture Summary

- **Temperature** matters when many completions are valid or when you need stability
- **Max tokens** caps length, cutoff risk, and **cost**
- **Stop sequences** end generation at a template boundary
- **Frequency** fights repeats; **presence** pushes new content
- **Tuning** is comparing bundles on the same examples, not guessing a magic number
- **Practical value:** You can sit with a PM and justify why the FAQ bot is not the slogan bot
