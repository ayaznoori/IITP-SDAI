# Pre-Read: Intro to AI & Prompt Engineering

## Session Mindmap

This diagram shows where this session sits in the course.

```mermaid
flowchart TB
    subgraph foundation ["Foundation"]
        direction TB
        P1["<b>Previous Module</b><br/>Module 4: AI Coding Partner<br/><i>[Cursor · Hygiene]</i><br/>Review AI code, small diffs"]
        P2["<b>Previous Module</b><br/>Module 9: Deploy Ops<br/><i>[CI · Secrets]</i><br/>Keys stay off the client"]
        CM["<b>Current Module Until Previous Session</b><br/>Module 10: Software 3.0<br/><i>[Starts here]</i><br/>Need builder LLM literacy"]
    end

    subgraph current ["Current Session"]
        CS["<b>Current Session</b><br/>Intro to AI & Prompt Engineering<br/><i>Mental shift:</i> from <b>casual chat</b> to <b>designed messages</b><br/>Tokens · system/user · few-shot"]
    end

    subgraph value ["Course & Real-Life Value"]
        CV["<b>Course Value</b><br/>Prompts become FastAPI product code"]
        RL["<b>Real-Life Use</b><br/>Support bots · writing tools · Copilot-like UX"]
    end

    subgraph future ["Upcoming Modules"]
        direction TB
        U1["<b>Upcoming Module</b><br/>Module 10 continues<br/><i>[LLM APIs]</i><br/>Python plus React wrapper"]
        U2["<b>Upcoming Module</b><br/>Module 11: Industry Spotlight<br/><i>[Specs · RAG]</i><br/>When models need your data"]
        U3["<b>Upcoming Module</b><br/>Module 12: Capstone<br/><i>[Product build]</i><br/>AI only if the spec needs it"]
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

- What an **LLM** is at a **builder** level — a next-token machine you call with text
- **Tokens** and the **context window** in simple words
- How to write a **system prompt** and a **user message**
- **Zero-shot** vs **few-shot** on a small task
- How to **rewrite a weak prompt** into a clearer one

---

## 2. Detailed Explanation

### LLM, Builder Level

An **LLM** (Large Language Model) predicts likely next pieces of text from the text you give it. It is not a search engine and not a guaranteed truth source.

You already used AI in Cursor. Today you name the parts so you can **design** the call, not only chat.

**Analogy:** An autocomplete that can follow instructions for a whole paragraph, not just one word.

> **In the Real World:** **ChatGPT**, **Claude**, and **GitHub Copilot** are product skins on this idea. Builders at **Notion AI** and **Intercom** write prompts as product code.

**Why It Matters**

- Features will wrap models, not only chat tabs
- Bad prompts waste tokens and confuse users
- You must know limits before you trust output

**Benefits**

- Shared vocabulary with AI teams
- Better Cursor prompts too
- Prep for the next session’s API call

### Tokens and Context Window

A **token** is a chunk of text the model reads or writes (often a short word piece).

The **context window** is the maximum tokens of **input + output** the model can hold in one go. Long chats fill it. Old turns may drop.

**Simple picture:** A backpack. Tokens are items. When the backpack is full, something falls out.

You do not need a tokenizer library today. You need the idea: **shorter, clearer prompts cost less and fit better**.

### System Prompt vs User Message

- **System prompt:** standing orders — role, rules, format
- **User message:** this request — the task and the data

```text
System: You extract task titles. Reply as one JSON object with key title.
User: Buy milk tomorrow
```

### Zero-Shot and Few-Shot

**Zero-shot:** instructions only, no examples.

**Few-shot:** instructions plus two or three example pairs.

Use few-shot when format is picky (JSON, tags, tone).

### Improve a Weak Prompt

**Weak:** "Do this better."

**Clear:** "Rewrite this email in 3 sentences. Keep the meeting time. Formal tone. Output only the email."

**Messy to Clear**

**Messy:** One blob of chat with no role and no output shape.

**Clear:** System rules + user data + examples if needed.

> **In the Real World:** **Support** tools at **Zendesk**-class products encode tone in the system prompt so every agent reply is on-brand.

### Building Blocks Checklist

- [ ] I can define LLM without saying “it thinks”
- [ ] I can explain token and backpack/window
- [ ] I can split system vs user
- [ ] I can spot zero-shot vs few-shot
- [ ] I can tighten a vague prompt

---

## 3. Practice Exercises

**Exercise 1 — Builder definition**
Write one sentence: an LLM predicts next tokens from context.

**Exercise 2 — Window**
Why might a very long paste fail or ignore the start?

**Exercise 3 — Split**
Turn “You are a tutor. Explain loops.” into system vs user.

**Exercise 4 — Shots**
Write two example pairs for “classify: bug or feature.”

**Exercise 5 — Rewrite**
Improve: “Make notes.” Add audience, length, and format.
