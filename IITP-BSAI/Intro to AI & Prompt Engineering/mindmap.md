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
