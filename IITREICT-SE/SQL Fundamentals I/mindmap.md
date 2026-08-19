```mermaid
flowchart TB
    subgraph foundation ["Foundation"]
        direction TB
        P1["<b>Previous Module</b><br/>Module 1: Developer Setup<br/><i>[Python · JSON files]</i><br/>In-memory vs disk data"]
        P2["<b>Previous Module</b><br/>Module 2: Web Fundamentals<br/><i>[APIs as contracts]</i><br/>Resources need storage"]
        CM["<b>Current Module Until Previous Session</b><br/>Module 3: FastAPI Backend<br/><i>[FastAPI · Files]</i><br/>APIs still use Python lists"]
    end

    subgraph current ["Current Session"]
        CS["<b>Current Session</b><br/>SQL Fundamentals I<br/><i>Mental shift:</i> from <b>lists that vanish</b> to <b>tables that last</b><br/>PK · FK · SELECT · INSERT"]
    end

    subgraph value ["Course & Real-Life Value"]
        CV["<b>Course Value</b><br/>Schema skill before ORM<br/>Capstone database design"]
        RL["<b>Real-Life Use</b><br/>Users · orders · tickets in Postgres or SQLite"]
    end

    subgraph future ["Upcoming Modules"]
        direction TB
        U1["<b>Upcoming Module</b><br/>Module 4: LLM & OpenAI APIs<br/><i>[Python · Data]</i><br/>Persist AI feature logs later"]
        U2["<b>Upcoming Module</b><br/>Module 5: AI-First Development<br/><i>[Specs]</i><br/>Schema in PRDs"]
        U3["<b>Upcoming Module</b><br/>Module 7: Capstone<br/><i>[FastAPI · DB]</i><br/>Product tables in production"]
    end

    P1 ==>|&nbsp;Foundation&nbsp;| P2
    P2 ==>|&nbsp;Need Store&nbsp;| CM
    CM ==>|&nbsp;Builds on&nbsp;| CS
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
