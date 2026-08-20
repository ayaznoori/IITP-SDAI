# Pre-Read: Masterclass — AWS Deployment

## Session Mindmap

This diagram shows where this session sits in the course.

```mermaid
flowchart TB
    subgraph foundation ["Foundation"]
        direction TB
        P1["<b>Previous Module</b><br/>Module 6: Backend FastAPI<br/><i>[API · CORS]</i><br/>What you actually ship"]
        P2["<b>Previous Module</b><br/>Module 7: Database<br/><i>[Neon · ORM]</i><br/>Data lives outside the box"]
        CM["<b>Current Module Until Previous Session</b><br/>Module 9: Deploy Ops<br/><i>[Docker · Actions]</i><br/>Image, CI, PaaS URL"]
    end

    subgraph current ["Current Session"]
        CS["<b>Current Session</b><br/>Masterclass: AWS Deployment<br/><i>Mental shift:</i> from <b>one PaaS button</b> to <b>a cloud map</b><br/>Outline · secrets · checklist"]
    end

    subgraph value ["Course & Real-Life Value"]
        CV["<b>Course Value</b><br/>Honest production literacy"]
        RL["<b>Real-Life Use</b><br/>Interviews · safer deploys · later scale"]
    end

    subgraph future ["Upcoming Modules"]
        direction TB
        U1["<b>Upcoming Module</b><br/>Module 10: Software 3.0<br/><i>[Prompts · LLM APIs]</i><br/>AI features on shipped backends"]
        U2["<b>Upcoming Module</b><br/>Module 11: Industry Spotlight<br/><i>[Specs · RAG]</i><br/>Product thinking plus retrieval"]
        U3["<b>Upcoming Module</b><br/>Module 12: Capstone<br/><i>[Build · Deploy]</i><br/>Public product story"]
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

- **AWS at a beginner high level** — a toolbox of cloud services, not one button
- A **simple outline** for deploying a containerised backend on AWS
- How to **treat env vars and secrets** as sensitive, not as files in Git
- **When Render/Railway is enough** versus when AWS shows up in jobs
- A **short production deploy checklist** you can reuse

---

## 2. Detailed Explanation

### What Is AWS, Plainly?

**AWS** (Amazon Web Services) is a large catalog of rented computers, databases, networks, and extras. You pick pieces. You pay for what you use.

You already used **PaaS** (Platform as a Service — Render/Railway hide servers). AWS is closer to assembling Lego. More power. More ways to get lost.

**Analogy:** Railway is a furnished studio. AWS is a hardware store plus an empty plot.

> **In the Real World:** **Netflix**, **Airbnb**, and huge Indian banks run on AWS or similar clouds. Interns rarely start by inventing a VPC. They still must **speak the names** in interviews.

**Why It Matters**

- Job descriptions list AWS even for junior roles
- You should not fear the console
- You should not pretend you “know AWS” after one click

**Benefits**

- Honest comparison with PaaS
- Safer secret habits
- A checklist that prevents “demo-only” deploys

### Outline: Containerised Backend on AWS (Bird’s Eye)

Stay at outline level:

1. Put the **Docker image** in a registry (AWS has one; others exist).
2. Run that image on a **compute** service that understands containers.
3. Give it **env vars** (database URL).
4. Put a **public HTTPS URL** in front.
5. Watch logs and a simple health check.

You will **not** configure every network knob in this masterclass. You will be able to **tell the story** in order.

### Env Vars and Secrets

A **secret** is a value that grants access: Neon password, API tokens.

Rules you already started:

- Not in Git
- Not in screenshots in Slack
- Different values per environment

AWS adds **managed secret stores** in real companies. Beginner version: platform env vars + least people who can read them.

> **In the Real World:** One leaked AWS key on GitHub can mine crypto on your bill overnight. This is not a scare story for fun. It is a weekly incident class.

### Render/Railway vs AWS for Early Projects

| Early project | Render / Railway | AWS |
|---------------|------------------|-----|
| Time to first URL | Short | Longer |
| Mental load | Low | High |
| Resume keyword | “Deployed API” | “AWS” if you truly used it |
| Best use now | Ship the BSAI backend | Learn map + checklist |

For this course, **PaaS is the default ship path**. AWS is literacy and a later option.

### Short Production Checklist

- [ ] App starts from a container or documented start command
- [ ] `DATABASE_URL` and secrets not in the repo
- [ ] HTTPS URL opens `/docs` or a health route
- [ ] Logs are reachable when it fails
- [ ] You know who can change env vars

**Messy to Clear**

**Messy:** “I deployed on AWS” with no URL and a committed key.

**Clear:** PaaS or AWS, live URL, secrets off Git, checklist ticked.

### Building Blocks Checklist

- [ ] I can define AWS in one sentence
- [ ] I can list the five outline steps
- [ ] I can explain a secret vs a normal setting
- [ ] I can argue PaaS vs AWS for a student app
- [ ] I can recite the checklist

---

## 3. Practice Exercises

**Exercise 1 — Definition**
Explain AWS to a friend who only used Render. Four sentences max.

**Exercise 2 — Outline**
Number the five bird’s-eye deploy steps from memory.

**Exercise 3 — Secrets**
Give two places a database password must **not** live.

**Exercise 4 — Compare**
When would you keep a student project on Railway instead of AWS?

**Exercise 5 — Checklist**
Tick which item you can already prove with your Render/Railway URL.
