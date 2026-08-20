# Pre-Read: Human-AI Workflows & Portfolio Readiness

## Session Mindmap

This diagram shows where this session sits in the course.

```mermaid
flowchart TB
    subgraph foundation ["Foundation"]
        direction TB
        P1["<b>Previous Module</b><br/>Module 4: AI Coding Partner<br/><i>[Review · Small diffs]</i><br/>Hygiene you must revive"]
        P2["<b>Previous Module</b><br/>Module 9: Deploy Ops<br/><i>[Live URL · CI]</i><br/>Something public to defend"]
        CM["<b>Current Module Until Previous Session</b><br/>Module 11: Industry Spotlight<br/><i>[Specs]</i><br/>You know what done means"]
    end

    subgraph current ["Current Session"]
        CS["<b>Current Session</b><br/>Human-AI Workflows & Portfolio Readiness<br/><i>Mental shift:</i> from <b>generated code</b> to <b>owned evidence</b><br/>Verify · README · hiring pitch"]
    end

    subgraph value ["Course & Real-Life Value"]
        CV["<b>Course Value</b><br/>Capstone starts from a clean story"]
        RL["<b>Real-Life Use</b><br/>Interviews · internships · trust"]
    end

    subgraph future ["Upcoming Modules"]
        direction TB
        U1["<b>Upcoming Module</b><br/>Module 11 continues<br/><i>[RAG ideas]</i><br/>Why private context needs retrieval"]
        U2["<b>Upcoming Module</b><br/>Module 12: Capstone<br/><i>[FE · BE build]</i><br/>Core screens and APIs"]
        U3["<b>Upcoming Module</b><br/>Module 12: Capstone<br/><i>[Deploy · Demo]</i><br/>Public URL and quality gate"]
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

- How to **review AI output at each step** before you continue
- How to **catch and fix at least one AI mistake**
- How to **polish a README and commit history**
- How to **confirm a live deployment URL** still works
- How to **package the project** for a hiring conversation

---

## 2. Detailed Explanation

### AI Is a Fast Junior

Cursor can write a route in seconds. It can also invent a field you never specified.

A **human-AI workflow** means: generate → **read** → run → **then** next step. Not generate → generate → generate.

**Analogy:** A sous-chef. You still taste the sauce.

> **In the Real World:** **Google** and **Microsoft** publish AI-use rules for staff: review before merge. **GitHub Copilot** PRs that nobody read are a known failure mode.

**Why It Matters**

- Interviews will probe whether you understand your repo
- Broken live URLs kill trust in seconds
- Commits and README are the first impression

**Benefits**

- Fewer silent bugs
- A story you can tell out loud
- Proof you used AI without being used by it

### Review at Each Step

Checklist per AI change:

- Does it match the spec?
- Did I run it?
- Did I keep the diff small?

Reject the rest.

### Catch One Mistake

Typical AI mistakes: fake imports, wrong field names, keys in examples, tests that never assert.

You will be given (or will find) one bug. You fix it. That is the LO.

### README and Commits

**README:** what it is, how to run, env var **names**, live URL.

**Commits:** small, honest messages. Not one dump called `final`.

### Live URL

Click it on a phone or incognito. `/docs` or the React app. If it sleeps, note cold start in the README.

### Package for Hiring

One paragraph: problem, your stack, what you built, URL, what you would do next. Practise saying it in 60 seconds.

**Messy to Clear**

**Messy:** 40 files you cannot explain, dead Vercel link, `asdf` commits.

**Clear:** Spec-sized feature, green CI, live URL, README, story.

> **In the Real World:** **Stripe** interviews often start at the GitHub profile. Hygiene is the handshake.

### Building Blocks Checklist

- [ ] I pause after each AI step
- [ ] I have found one AI mistake before
- [ ] README has run steps and URL
- [ ] I clicked the live URL this week
- [ ] I can pitch in one minute

---

## 3. Practice Exercises

**Exercise 1 — Loop**
Write the four words: generate, read, run, next.

**Exercise 2 — Mistake**
List two AI bugs you would catch by running Swagger.

**Exercise 3 — README**
Draft the “Run locally” section with venv and `uvicorn` in three lines.

**Exercise 4 — URL**
Open your deploy URL now. Write the status code you saw.

**Exercise 5 — Pitch**
Write a 4-sentence hiring paragraph for your CRUD or AI wrapper.
