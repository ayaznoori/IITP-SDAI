# Pre-Read: LLM Foundations & Prompt Engineering

## 1. What You'll Learn

In this pre-read, you'll discover:

- What a **Large Language Model (LLM)** is and how it predicts the next piece of text
- How **tokens**, **context windows**, and **knowledge cutoff** shape what the model can see
- Why the **same prompt** can produce **different outputs**
- How to write **zero-shot** and **few-shot** prompts for product tasks
- How **role prompting** and **structured output** make answers more consistent

---

## 2. Detailed Explanation

### What Is an LLM?

A **Large Language Model** is a program trained on huge amounts of text. It does not "look up" a stored answer like a database. It **predicts likely next tokens** from the text you give it.

**One-line definition:** An LLM is a next-token predictor that turns your prompt into a continuation.

**Analogy:** Think of a very fast autocomplete. Your phone suggests the next word. An LLM suggests many next words, one after another, until it finishes a reply.

> **In the Real World:** **ChatGPT**, **Gmail Smart Compose**, and **Notion AI** all sit on this idea. Product teams do not treat the model as a search engine. They treat it as a **text generator** they must steer.

### Tokens

A **token** is a chunk of text the model actually reads — often a word, part of a word, or punctuation.

Examples (intuition, not exact counts):
- `"hello"` → often 1 token
- `"ChatGPT"` → may split into more than one token
- `"₹499"` → numbers and symbols can use extra tokens

**Why It Matters:** APIs usually bill and limit by tokens. Long prompts plus long answers cost more and can hit limits.

**Benefits:**
- You can estimate cost before you ship a feature
- You can keep prompts short and useful
- You can leave room in the window for the model's reply

### Context Window

The **context window** is the maximum number of tokens the model can consider at once. That includes:
- your instructions
- user input
- any examples you paste
- the model's own reply (as it is generated)

**Analogy:** A whiteboard of fixed size. When you fill it, older notes get erased. The model cannot "see" text that fell off the board.

If your prompt is huge, the model may miss early instructions. If the user pastes a giant ticket, later lines may get truncated.

> **In the Real World:** Support tools at companies like **Intercom** and **Freshworks** must summarise tickets **inside** a window. Teams truncate old chat, keep the latest messages, and put the **task at the end** so it stays visible.

### Knowledge Cutoff

**Knowledge cutoff** is the date after which the model's training data stops. The model does not automatically know last week's news unless you put that fact in the prompt.

**Messy:** "Who won yesterday's IPL match?" with no data in the prompt.  
**Clear:** Paste the match result, then ask for a one-line recap.

The model can still **reason** over text you provide. It cannot magically browse the live web in this session's model of how LLMs work.

### Why Outputs Vary

LLMs are **not calculators**. Same prompt, slightly different wording in the reply is common because generation is **sampling** from many possible next tokens.

Other reasons answers change:
- Tiny prompt edits change the path
- Hidden system instructions (in a product) differ
- The model is asked an **open** task ("write a tagline") vs a **closed** one ("return JSON with two keys")

**Mental model:** A chef with a recipe that still allows seasoning. Tight recipes (format, examples, role) reduce surprise. Loose recipes increase variety.

> **In the Real World:** **Duolingo** and **Grammarly** need **stable** corrections. A banking FAQ bot needs the **same** policy wording. A marketing slogan tool may **want** variety. You choose stability vs variety with prompt design (next session adds temperature).

### Zero-Shot Prompts

**Zero-shot** means: give the task, give no examples.

```
Summarise this customer email in one sentence.
Email: "My order #8821 arrived without the charger. Need a replacement today."
```

Use zero-shot when the task is obvious and the format is simple.

### Few-Shot Prompts

**Few-shot** means: show a few **input → output** examples, then the real input.

```
Rewrite support replies to be short and kind.

Email: "This app is garbage."
Reply: "Sorry this felt frustrating. What went wrong so we can help?"

Email: "Where is my refund?"
Reply: "Refunds take 5–7 days. Share your order ID and we will check."

Email: "The screen is cracked."
Reply:
```

The model copies the **pattern**: short, kind, useful.

> **In the Real World:** **Swiggy** or **Zomato**-style support macros often start as few-shot examples of "good replies" so the tone stays on-brand.

### Role Prompting

**Role prompting** sets who the model should act as. That steers vocabulary and priorities.

```
You are a careful pharmacy assistant.
Only use the facts in the customer message.
If dose information is missing, say you cannot advise.
Do not invent medicine names.
```

Roles are not magic titles. Pair them with **rules** and **allowed sources** (the text in the prompt).

### Structured Output

**Structured output** means you demand a format: JSON, a numbered list, or fixed headings. Products parse structure in code.

```
Return JSON only, no markdown:
{"intent": "refund" | "shipping" | "other", "urgency": "low" | "high"}
```

**Small code-shaped example (max 10 lines) — a prompt template you could store in a file:**

```text
You are a ticket classifier for an online store.
Use only the email text.
Return JSON: {"intent": "...", "urgency": "low|high"}
Email:
{{email_text}}
```

Replace `{{email_text}}` with the real message in your app later.

### Refine for Consistency

Unstable prompts are vague: "help the user." Stable prompts name:
- **Goal** (classify, summarise, rewrite)
- **Constraints** (one sentence, no medical advice)
- **Format** (JSON keys)
- **Examples** (few-shot) when the style is subtle
- **Fallback** ("If unclear, set intent to other")

**Messy to clear walkthrough**

Messy prompt: `Fix this.`  
Clear prompt: `You are a copy editor. Rewrite the review in 20 words. Keep the rating. Output only the rewrite.`

**Building blocks**
- Task sentence
- Role + rules
- Examples (optional)
- Output schema
- User content last (so it stays in the window)

### Why It Matters

If you cannot explain tokens, windows, and cutoff, you will blame the model for "being dumb" when you actually overflowed the board or asked about news it cannot know. If you cannot write zero-shot, few-shot, role, and structured prompts, your FastAPI AI feature (coming sessions) will feel random.

**Benefits:**
- Predictable product copy and classifications
- Easier parsing in Python
- Less user confusion from wild answers

---

## 3. Practice Exercises

**Exercise 1 — Token intuition**  
Count words in: `Order 8821 needs a charger`. Is the token count likely equal to the word count? Write one sentence why or why not.

**Exercise 2 — Cutoff vs context**  
A user asks for "today's gold price." You have no live feed. Write a one-line assistant rule that refuses guessing.

**Exercise 3 — Zero-shot**  
Write a zero-shot prompt that turns a product review into exactly three bullet strengths. No extra commentary.

**Exercise 4 — Few-shot**  
Write two examples that map a messy address to `{"city": "...", "pincode": "..."}`. Then leave a third address blank for the model.

**Exercise 5 — Role + structure**  
Write a role prompt for a "library desk clerk." Demand JSON: `{"can_borrow": true/false, "reason": "..."}` based only on a pasted membership note.
