# Pre-Read: AI Coding Agents & Personal Workflow

## Session Mindmap

This diagram shows where this session sits in the course.

```mermaid
%%{init: {'flowchart': {'nodeSpacing': 42, 'rankSpacing': 54, 'padding': 18}}}%%
flowchart TB
    subgraph foundation ["Foundation"]
        direction TB
        P1["<b>Previous Module</b><br/>Module 1: Developer Setup<br/><i>[Git · PRs]</i><br/>Reviewable history"]
        CM["<b>Current Module Until Previous Session</b><br/>Module 5: AI-First Development<br/><i>[Copilot · Specs]</i><br/>Context packing · PRDs · small reviews"]
    end

    subgraph current ["Current Session"]
        CS["<b>Current Session</b><br/>AI Coding Agents and Personal Workflow<br/><i>Mental shift:</i> from <b>ad-hoc chat</b> to <b>plan-then-patch discipline</b><br/>Plan gate · small diffs · PR checklist · playbook"]
    end

    subgraph value ["Course and Real-Life Value"]
        CV["<b>Course Value</b><br/>Closes Module 5 ready for shipping"]
        RL["<b>Real-Life Use</b><br/>Cursor agents · still human-owned PRs"]
    end

    subgraph future ["Upcoming Modules"]
        direction TB
        U1["<b>Upcoming Module</b><br/>Module 6: Shipping AI Apps<br/><i>[Docker · LLMOps]</i><br/>Same control in production"]
        U2["<b>Upcoming Module</b><br/>Module 7: Capstone Product<br/><i>[Agents · Full stack]</i><br/>Workflow you wrote, reused"]
    end

    P1 ==>|&nbsp;Foundation&nbsp;| CM
    CM ==>|&nbsp;Builds On&nbsp;| CS
    CS ==>|&nbsp;Course Path&nbsp;| CV
    CS ==>|&nbsp;Real-Life Use&nbsp;| RL
    CS ==>|&nbsp;Next Module&nbsp;| U1
    U1 -.-> U2

    classDef previous fill:#E8F4FD,stroke:#4A90D9,stroke-width:2px,color:#1a1a1a
    classDef current fill:#FFF3CD,stroke:#E6A817,stroke-width:3px,color:#1a1a1a
    classDef value fill:#D4EDDA,stroke:#28A745,stroke-width:2px,color:#1a1a1a
    classDef future fill:#F3E8FF,stroke:#9B59B6,stroke-width:2px,color:#1a1a1a

    class P1,CM previous
    class CS current
    class CV,RL value
    class U1,U2 future

    linkStyle default stroke-width:3px
```

## 1. What You'll Learn

In this pre-read, you'll discover:

- How to run a workflow where the **agent proposes a plan** before coding
- How to use **clarifying questions** and keep work as **small diffs**
- How to **review AI changes** with a **PR-style checklist**
- How to **stay in control** instead of accepting huge unreviewed edits
- How to build a **personal, repeatable AI-first workflow**

---

## 2. Detailed Explanation

### Agents vs Pair Completions

**Copilot grey text** completes a few lines. A **coding agent** can edit many files, run commands, and loop. Power without a workflow is how repos get wrecked.

**One-line definition:** An AI coding agent is a helper that should **plan, ask, patch small, and wait for review**.

**Analogy:** A junior on a first PR. They do not rewrite the company. They propose a plan, ask two questions, change five lines, and request review.

> **In the Real World:** **Cursor** agents, **Claude Code**, and similar tools are used at startups and enterprises. Teams that skip plans get "drive-by refactors." **GitHub** still wants pull requests. The agent is not exempt.

### Plan Before Coding

Your first message should demand a plan:

```
Read schemas.py and classify route only.
Goal: add summary field from last spec.
Write a 5-step plan. Do not edit files yet.
Ask if anything in the spec is ambiguous.
```

Approve or correct the plan. **Then** allow edits.

### Clarifying Questions and Small Diffs

Good agents ask:
- Placeholder summary vs extra LLM call?
- Which files are in bounds?

**Small diff** means: one intent per change set. Easy to revert. Easy to review.

**Messy:** 18 files, rename everything, "while I was here."  
**Clear:** schema + route + one test.

### PR-Style Review Checklist

Treat agent output like a teammate's PR:

- [ ] Matches the approved plan
- [ ] No secret files touched
- [ ] Validation and DATA fences still there
- [ ] Tests added/updated and **run**
- [ ] Diff size reasonable
- [ ] You can explain each hunk

If you cannot explain it, it does not merge.

### Stay in Control

**You** decide when the agent may write. Stop words: "pause," "show diff only," "revert file X."

Reject **large unreviewed edits** even if tests pass. Passing tests can still delete comments, logging, or security.

**Control habits:**
- Branch first (`git checkout -b feat/summary`)
- Commit working state before the agent
- One task per agent run
- Read the diff in the editor, not the chat summary

### Personal Repeatable AI-First Workflow

Write a short playbook you will reuse:

1. Spec / ticket (from last session)
2. Pack context (files + constraints)
3. **Plan only**
4. Answer clarifying questions
5. Agent implements **small diff**
6. You run tests and PR checklist
7. Commit with a human message
8. Next task — new plan

Use it for **design** (spec), **implementation** (code), and **debugging** (plan: "add logs / write failing test / fix").

**Why It Matters:** Capstone week will tempt you to "let the agent finish." This session is the seatbelt.

**Benefits:**
- Reversible work
- Learnable history
- Security preserved
- You can describe your workflow in interviews

### Building Blocks

- Plan gate
- Questions
- Small diffs
- PR checklist
- Personal playbook

---

## 3. Practice Exercises

**Exercise 1 — Plan prompt**  
Write a prompt that forbids file edits until you say "go." Include the goal in one sentence.

**Exercise 2 — Questions**  
List two clarifying questions an agent should ask before adding `summary`.

**Exercise 3 — Small vs large**  
Which is a small diff: (a) schema + route + test, or (b) rewrite all routers to async plus new folder structure? Why?

**Exercise 4 — Checklist**  
Add one checklist item that specifically protects Module 4 **prompt-injection** defences.

**Exercise 5 — Playbook**  
Number your 6-step personal workflow from ticket to commit. Keep it generic so you can reuse it.
