# Lecture Script: Specs & UI Design with AI
**Duration:** 110 minutes | **Tools:** Chat AI, whiteboard/paper, existing `/ai/classify` contract | **Context:** Context engineering (only attach the real API)

---

## Session Timeline Overview

| Block | Time | Focus |
|-------|------|-------|
| Opening & hook | 5 min | Intercom-sized hallucination |
| Why Does This Matter? | 10 min | PRDs in industry |
| What Is the Concept? | 20 min | Brief, stories, contracts, flows |
| How Do We Apply It? (LOs) | 55 min | Bookstore feature workshop |
| Refine clinic | 15 min | Cut scope, make testable |
| Recap | 5 min | Agents consume specs |

---

## Session Opening (5 min)

**[Script:]** "If you ask AI to 'design a support product,' it will invent **WhatsApp**, **voice**, and **agent desktops**. Today you draft a **PRD**, **stories**, **API contract**, and **UI flow** for **one** feature you already have — then **you** refine. Specs are product engineering."

**Problem hook:** Show an inflated AI PRD. Red-pen 80%. "This is the skill."

---

## Why Does This Matter?

🎯 **Instructor Note:** Ask who has built the wrong feature because the ticket was vague.

**[Script:]** "**Amazon** six-pagers, **Spotify** feature briefs, **CRED** and **Razorpay** tickets in Jira — same idea. Designers at **Figma**-using teams still need **acceptance criteria**. AI is a junior PM. You are the editor-in-chief."

**Pain if misunderstood:**
- Engineers implement ghost endpoints
- QA has nothing testable
- UI assumes fields the API never returns
- Capstone scope explodes

---

## What Is the Concept?

### PRD / Feature Brief

Problem, user, goals, **non-goals**, constraints.

### User Stories and AC

Role, want, why. AC must be pass/fail.

### API Contracts and Edge Cases

JSON + status codes + nasty inputs (from Module 4 security).

### UI Flows / Wireframes

Ordered screens. Empty, loading, success, error.

### Implementable Requirements

No adjectives without numbers ("fast" → "show result or error in the UI after submit").

---

## How Do We Apply It?

### LO 1: Draft PRD/feature brief with AI then human refine

**Prompt (pack context: actual `TicketIn` / `TicketOut`):**

```
Draft a 1-page brief: Agent Ticket Helper for a bookstore.
Must use existing classify API. Non-goals: no auth UI, no DB, no mobile.
```

**Human refine live:** delete extra personas, add max 2000 chars, add "no stack traces."

**Predict:** How many features did AI add beyond classify?

**Explain result:** Draft is clay. Refine is the product.

> **In the Real World:** **Notion** templates for PRDs still get a human pass.

---

### LO 2: User stories and acceptance criteria with AI support

**Ask AI for 8 stories. Keep 3:**
- Classify happy path
- Validation error
- AI unavailable

**Rewrite AC** until each is Given/When/Then.

**Predict:** Is "should feel snappy" an acceptance criterion?

**Explain result:** No. Not testable.

---

### LO 3: Generate API contracts and edge cases

**Table on board** (AI drafts, class corrects):

| Case | Input | Expected |
|------|-------|----------|
| OK | 1–2000 chars | 200 + allow-listed labels |
| Empty | `""` | 422 |
| Huge | 2001 chars | 422 |
| Injection | ignore-instructions text | still allow-listed JSON |
| Provider down | — | 503 generic detail |

**Predict:** Did AI invent `GET /ai/classify/{id}`?

**Explain result:** Cut ghosts. Contract matches FastAPI.

---

### LO 4: Draft UI flows/wireframes with AI

**Text wireframe:**

```
[ textarea ][ Classify ]
loading: spinner
success: intent | urgency | summary
error: banner
```

Paper sketch 4 boxes. Name **empty state** copy.

**Predict:** What happens if we forget loading?

**Explain result:** Double-submit, extra LLM cost (Module 4). Flow must include waiting.

---

### LO 5: Convert design notes into implementable requirements

**Messy notes → numbered reqs:**

1. POST body `{ ticket_text }`
2. Disable button while in flight
3. Render three fields from `TicketOut`
4. Map 422 field errors to inline text
5. Map 503 to generic banner

**Predict:** Can a teammate implement without asking you?

**Explain result:** If yes, the spec is implementable.

---

## Refine Clinic (15 min)

Pairs: one AI-raw spec, one editor. Editors must produce a **non-goals** list of at least five cuts.

---

## Recap (5 min)

**[Script:]** "Next session an **agent** may code from this brief. If the brief is sloppy, the agent will happily build WhatsApp."

---

## Lecture Summary

- AI **drafts** PRDs; humans **refine** scope and non-goals
- **Stories + acceptance criteria** must be testable
- **API contracts and edge cases** include 422, 503, and hostile text
- **UI flows** cover empty, loading, success, error
- Convert notes into **numbered implementable requirements**
- **Practical value:** You can brief a teammate or an agent without exploding scope
