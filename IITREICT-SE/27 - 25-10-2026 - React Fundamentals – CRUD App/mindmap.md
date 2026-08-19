```mermaid
flowchart TB
    subgraph foundation ["Foundation"]
        P1["<b>Previous Module</b><br/>Module 2: Web Fundamentals<br/><i>[Vanilla Fetch]</i><br/>GET and render without React"]
        P2["<b>Previous Module</b><br/>Module 2: Web Fundamentals<br/><i>[React theory]</i><br/>JSX · props · two hooks"]
        CM["<b>Current Module Until Previous Session</b><br/>Module 2: Web Fundamentals<br/><i>[Vite React basics]</i><br/>Counters and effects ready for CRUD"]
    end
    subgraph current ["Current Session"]
        CS["<b>Current Session</b><br/>React Fundamentals: CRUD App<br/><i>Mental shift:</i> <b>demo widgets</b> → <b>a persistable product</b><br/>todo CRUD · localStorage · one GET · loading"]
    end
    subgraph value ["Course & Real-Life Value"]
        CV["<b>Course Value</b><br/>Closes Module 2 with a take-home shaped app"]
        RL["<b>Real-Life Use</b><br/>Tasks apps · intern CRUD screens · offline notes"]
    end
    subgraph future ["Upcoming Modules"]
        U1["<b>Upcoming Module</b><br/>Module 3: FastAPI<br/><i>[REST · venv]</i><br/>Your own GET/POST endpoints"]
        U2["<b>Upcoming Module</b><br/>Module 3: FastAPI<br/><i>[SQL · Auth]</i><br/>Real persistence and login"]
        U3["<b>Upcoming Module</b><br/>Module 4: LLM APIs<br/><i>[OpenAI]</i><br/>Add AI features on the same stack"]
    end
    P1 ==>|&nbsp;Foundation&nbsp;| P2
    P2 ==>|&nbsp;Components&nbsp;| CM
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
