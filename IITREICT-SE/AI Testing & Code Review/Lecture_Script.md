# Lecture Script: AI Testing & Code Review
**Duration:** 110 minutes | **Tools:** pytest or TestClient, git diff, chat AI | **Context:** FastAPI classify + eval habits

---

## Session Timeline Overview

| Block | Time | Focus |
|-------|------|-------|
| Opening & hook | 5 min | assert True |
| Why Does This Matter? | 10 min | Merge button |
| What Is the Concept? | 20 min | Ideas vs green lies |
| How Do We Apply It? (LOs) | 55 min | Generate, run, review |
| Ownership round | 15 min | Accept/reject aloud |
| Recap | 5 min | Multimodal next |

---

## Session Opening (5 min)

**[Script:]** "Yesterday we scored **prompts**. Today we score **code**. AI will invent tests and PR comments. Some will be gold. Some will be `assert True`. You **run**, **debug**, **apply useful refactors**, and **you** own merge."

**Problem hook:** Show a Copilot test file that never imports the app. "100% passed. 0% useful."

---

## Why Does This Matter?

🎯 **Instructor Note:** Ask who merged failing tests 'to fix later.'

**[Script:]** "**GitHub** Copilot Enterprise, **GitLab** suggested reviewers, **Qodo** and similar tools — the industry pattern is **AI draft, human gate**. **Razorpay** and **Stripe** outages are owned by people, not models. Your capstone README can mention tests you **ran**."

**Pain if misunderstood:**
- False confidence
- Tests coupled to hallucinations
- Rubber-stamp PR review
- Unreadable 'clever' refactors

---

## What Is the Concept?

### Test Ideas vs Test Code

Brainstorm first. Then generate. Then execute.

### Debug Loop

Failing test is a question: product or test wrong?

### PR Risk Review

Secrets, security, behaviour, scope.

### Readability

Apply only what you can teach back.

### Ownership

Record the decision.

---

## How Do We Apply It?

### LO 1: AI-generated test ideas and cases

**Paste** `TicketIn`, route, spec AC. Ask for a **table of ideas**, then pytest.

**Predict:** Will AI add a test for GraphQL?

**Explain result:** Cut ghosts. Keep 422/200/injection-adjacent.

> **In the Real World:** **Amazon** test plans start as cases, not as 400 generated files.

---

### LO 2: Run and debug AI tests before accepting

**Live:** run pytest. Pick one failure.

**Walkthrough:** expected 400 vs actual 422 — **fix the test** to match FastAPI.

Second failure: expected `Refund` — **fix production or test** to match allow-list lowercase.

**Predict before the run:** All generated tests pass on first try?

**Explain result:** Rarely. That is why we run.

🎯 **Instructor Note:** Use `TestClient` if pytest is already in the project; else a tiny script. Stay practical.

---

### LO 3: AI tools to review a PR and identify risks

**Prepare a dirty diff:** remove `max_length`, add a comment with `sk-demo`.

Ask AI to review. Compare to human list.

**Predict:** Did it catch the key?

**Explain result:** Good at patterns. Still **you** grep. Combine both.

---

### LO 4: Apply useful refactor/readability suggestions

**Ask** "rename `x` and extract allow-list check." Apply **one** hunk. Re-run tests.

Reject a suggestion that reformats 500 lines.

**Predict:** Do tests still pass after a pure rename?

**Explain result:** They should. If not, the refactor changed behaviour — revert.

---

### LO 5: Human ownership of accept/reject

Each student **writes** on the PR (or a note):
- Accepted: extra 422 test — matches spec
- Rejected: mock that skips validation — hides bugs

**Predict:** Can you blame Copilot in a postmortem?

**Explain result:** No. Your name is on the commit.

---

## Ownership Round (15 min)

Pairs: one AI review comment is wrong. Practice a polite reject sentence.

---

## Recap (5 min)

**[Script:]** "Last shipping session: **multimodal** — images in, still validate, still handle errors, still a product flow. Same ownership."

---

## Lecture Summary

- AI is strong at **test ideas and drafts** for existing features
- **Run and debug** tests before they enter git
- Use AI to **review PRs for risks**, then verify
- Take **readability** help when you understand it
- **Humans accept or reject** — quality is not outsourced
- **Practical value:** You can go faster on tests and review without lying to yourself with green fakes
