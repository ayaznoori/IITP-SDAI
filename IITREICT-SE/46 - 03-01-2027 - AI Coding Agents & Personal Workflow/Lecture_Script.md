# Lecture Script: AI Coding Agents & Personal Workflow
**Duration:** 110 minutes | **Tools:** Git, editor agent or chat-as-agent (instructor demo), FastAPI repo | **Context:** Spec + context packing from prior sessions

---

## Session Timeline Overview

| Block | Time | Focus |
|-------|------|-------|
| Opening & hook | 5 min | 18-file surprise diff |
| Why Does This Matter? | 10 min | PRs still exist |
| What Is the Concept? | 20 min | Plan, questions, small diffs |
| How Do We Apply It? (LOs) | 55 min | Live agent on summary field |
| Write your playbook | 15 min | Personal workflow doc |
| Recap | 5 min | Module 5 close |

---

## Session Opening (5 min)

**[Script:]** "Copilot was a pair. An **agent** can touch the whole tree. Without a workflow, it 'helps' by rewriting your project. Today: **plan first**, **questions**, **small diffs**, **PR checklist**, **you in control**, and a **repeatable personal workflow**."

**Problem hook:** Show `git diff --stat` with 18 files after "just add a summary." Class groans. "We will not do that."

🎯 **Instructor Note:** If no agent product is licensed, you play the agent: students may only send plan-first prompts; you return fake diffs.

---

## Why Does This Matter?

🎯 **Instructor Note:** Ask how they would revert a bad Copilot day without git.

**[Script:]** "**GitHub** PRs, **GitLab** MRs, **Bitbucket** — industry still reviews humans. Agents are extra humans. **Shopify** and **Stripe** engineering blogs keep repeating: small PRs. Your interview story should be: 'I run agents with a plan gate,' not 'I vibe-coded the capstone.'"

**Pain if misunderstood:**
- Unreviewable dumps
- Lost security fences
- No git archaeology
- You cannot explain the code you 'wrote'

---

## What Is the Concept?

### Plan Gate

No edits until plan approved.

### Clarifying Questions

Ambiguity is cheaper **before** code.

### Small Diffs

One story, few files.

### PR Checklist

Same as Module 1 PRs, plus AI-specific items (secrets, scope, tests run).

### Control vs Large Edits

You are tech lead. Agent is intern.

### Personal Workflow

Written, reused, improved.

---

## How Do We Apply It?

### LO 1: Agent proposes a plan before coding

**Live prompt:**

```
Do not edit. Plan steps to add TicketOut.summary per our spec.
Files allowed: schemas.py, classify route, test_classify.py.
```

**Predict:** Did it try to add Redis anyway?

**Explain result:** Plan is where you catch scope creep **cheaply**.

> **In the Real World:** **Atlassian** tickets with AC stop agents (and humans) from wandering.

---

### LO 2: Clarifying questions and small diffs

**Instructor plays PM:** "Is summary from a second model call or a stub?" Class answers. Agent may implement **only** that.

**Implement** on a git branch. `git diff` should be tiny.

**Predict:** How many files in `git diff --stat`?

**Explain result:** If more than ~3–4 without a reason, stop and split.

---

### LO 3: Review with a PR-style checklist

**Project checklist on screen.** Walk the diff hunk by hunk. Run tests **before** "looks good."

**Predict:** Can a green test still miss a removed `max_length`?

**Explain result:** Yes. Checklist includes security, not only pytest.

---

### LO 4: Stay in control vs large unreviewed edits

**Demo (or story):** Agent starts reformatting the whole repo. Student must say **stop**, `git checkout --` on unwanted files, or revert the commit.

**Predict:** Is "the agent is still running" a reason to skip reading?

**Explain result:** Never. Pause. Review. Then continue.

🎯 **Instructor Note:** Practice the stop. Muscle memory.

---

### LO 5: Build a personal repeatable AI-first workflow

**Students write `WORKFLOW.md` in their notes (not necessarily the product repo):** design → pack context → plan → questions → small diff → review → commit.

They must include **debug** path: reproduce, failing test, plan, fix.

**Predict:** Will they skip plan when tired?

**Explain result:** The doc exists **for tired you**. That is the point.

---

## Write Your Playbook (15 min)

Silent writing. Then 3 volunteers read their 6 steps. Class steals good lines.

---

## Recap (5 min)

**[Script:]** "Module 5 done. Module 6: **ship** the FastAPI app, **LLMOps**, testing/review, **multimodal**. Same control habits apply."

---

## Lecture Summary

- Agents must **propose a plan** before they edit
- **Clarifying questions** and **small diffs** keep work reviewable
- Review with a **PR-style checklist** and real test runs
- **Stay in control**; reject large unreviewed edits
- Write a **personal AI-first workflow** for design, implementation, and debug
- **Practical value:** You can use powerful agents without surrendering the repo
