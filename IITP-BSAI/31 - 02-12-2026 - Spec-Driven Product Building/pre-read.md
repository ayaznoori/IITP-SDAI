# Pre-Read: Spec-Driven Product Building

## Session Mindmap

This diagram shows where this session sits in the course.

```mermaid
flowchart TB
    subgraph foundation ["Foundation"]
        direction TB
        P1["<b>Previous Module</b><br/>Module 8: Testing Hygiene<br/><i>[Pytest · CRUD lab]</i><br/>You already felt 'done' as tests"]
        P2["<b>Previous Module</b><br/>Module 10: Software 3.0<br/><i>[Prompts · LLM APIs]</i><br/>AI needs a contract"]
        CM["<b>Current Module Until Previous Session</b><br/>Module 11: Industry Spotlight<br/><i>[Starts here]</i><br/>Need product language"]
    end

    subgraph current ["Current Session"]
        CS["<b>Current Session</b><br/>Spec-Driven Product Building<br/><i>Mental shift:</i> from <b>vague idea</b> to <b>testable slice</b><br/>Stories · AC · data sketch"]
    end

    subgraph value ["Course & Real-Life Value"]
        CV["<b>Course Value</b><br/>Capstone and AI stay in bounds"]
        RL["<b>Real-Life Use</b><br/>Tickets · PRDs · healthier vibe coding"]
    end

    subgraph future ["Upcoming Modules"]
        direction TB
        U1["<b>Upcoming Module</b><br/>Module 11 continues<br/><i>[Portfolio · RAG]</i><br/>Ship story plus retrieval idea"]
        U2["<b>Upcoming Module</b><br/>Module 12: Capstone<br/><i>[Build · Deploy]</i><br/>Spec becomes the brief"]
        U3["<b>Upcoming Module</b><br/>Hiring conversations<br/><i>[README · URL]</i><br/>Talk the slice you shipped"]
    end

    P1 ==>|&nbsp;Foundation&nbsp;| P2
    P2 ==>|&nbsp;Builds on&nbsp;| CM
    CM ==>|&nbsp;Blueprint&nbsp;| CS
    CS ==>|&nbsp;Course Path&nbsp;| CV
    CS ==>|&nbsp;Real-Life Use&nbsp;| RL
    CS ==>|&nbsp;Next Module&nbsp;| U1
    U1 -.-> U2
    U2 -.-> U3

    classDef previous fill:#E8F4FD,stroke:#4A90D9,stroke-width:2px,color:#1a1a1a
    classDef current fill:#FFF3CD,stroke:#E6A817,stroke-width:3px,color:#1a1a1a
    classDef value fill:#D4EDDA,stroke:#28A745,stroke-width:2px,color:#1a1a1a
    classDef future fill:#F3E8FF,stroke:#9B59B6,stroke-width:2px,color:#1a1a1a

    class P1,P2,CM previous
    class CS current
    class CV,RL value
    class U1,U2,U3 future

    linkStyle default stroke-width:3px
```

## 1. What You'll Learn

In this pre-read, you'll discover:

- **When to prototype fast** versus **when to write a spec first**
- How to write **user stories** for one small feature
- How **acceptance criteria** make “done” obvious
- How to **sketch a simple data model**
- How to turn a rough idea into a **short implementable spec**

---

## 2. Detailed Explanation

### Two Speeds

**Prototype fast** (sometimes called vibe coding): explore UI and ideas when you do not yet know the product.

**Specify first:** write stories and criteria when several people will build, or when AI will generate large chunks.

**Analogy:** Sketching a poster vs writing a building permit. Posters can be messy. Buildings need measurements.

> **In the Real World:** **Figma** prototypes explore. **Jira** stories ship. Strong teams use both on purpose, not by accident.

**Why It Matters**

- AI will happily generate the wrong app fluently
- Capstone and jobs need “done” definitions
- Data models prevent React/FastAPI drift (you felt this in CRUD lab)

**Benefits**

- Less rework
- Clear demos
- Better prompts because the spec is the prompt

### User Stories

Format: **As a** role, **I want** action, **so that** benefit.

Example: As a student, I want to add a task title, so that I do not forget lab homework.

One feature. Not the whole startup.

### Acceptance Criteria

Tests in English. If they fail, the story is not done.

Example:

- Given I am on the list, when I submit a non-empty title, then the task appears without a refresh.
- Empty title shows an error and does not POST.

### Simple Data Model Sketch

Boxes and fields. `Task: id, title, done`. You practised this with SQLAlchemy. The spec names the fields before code.

### Short Implementable Spec

One page:

1. Problem
2. User stories (2–3)
3. Acceptance criteria
4. Data model sketch
5. Out of scope (what you will not build)

**Messy to Clear**

**Messy:** “AI notes app like Notion.”

**Clear:** one user, one resource, criteria you could test in Swagger and the UI.

> **In the Real World:** **Spotify** two-week squads still write acceptance tests. Interns who can spec a slice get trusted sooner.

### Building Blocks Checklist

- [ ] I can pick prototype vs spec with a reason
- [ ] I can write one user story
- [ ] I can list two acceptance lines
- [ ] I can sketch fields
- [ ] I can keep a spec to one page

---

## 3. Practice Exercises

**Exercise 1 — Mode**
A weekend idea with no users yet — prototype or spec first? Why?

**Exercise 2 — Story**
Write one user story for “mark task done.”

**Exercise 3 — Criteria**
Write two Given/When/Then lines for that story.

**Exercise 4 — Model**
List fields for `Task`. Mark the id.

**Exercise 5 — Out of scope**
Name two features you will not build this week (auth, sharing).
