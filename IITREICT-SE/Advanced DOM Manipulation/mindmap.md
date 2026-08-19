```mermaid
flowchart TB
    subgraph foundation ["Foundation"]
        P1["<b>Previous Module</b><br/>Module 2: Web Fundamentals<br/><i>[HTML · CSS]</i><br/>Static tree in the file"]
        P2["<b>Previous Module</b><br/>Module 2: Web Fundamentals<br/><i>[JS · Async]</i><br/>Logic and timers"]
        CM["<b>Current Module Until Previous Session</b><br/>Module 2: Web Fundamentals<br/><i>[DOM select · events]</i><br/>Click and form on existing nodes"]
    end
    subgraph current ["Current Session"]
        CS["<b>Current Session</b><br/>Advanced DOM Manipulation<br/><i>Mental shift:</i> <b>edit nodes</b> → <b>grow the tree</b><br/>create/remove · classList · tabs/lists · DevTools"]
    end
    subgraph value ["Course & Real-Life Value"]
        CV["<b>Course Value</b><br/>Raw skill React will later wrap"]
        RL["<b>Real-Life Use</b><br/>Keep notes · cart rows · settings tabs"]
    end
    subgraph future ["Upcoming Modules"]
        U1["<b>Upcoming Module</b><br/>Module 2: Web Fundamentals<br/><i>[HTTP · Fetch · React]</i><br/>Fill lists from JSON"]
        U2["<b>Upcoming Module</b><br/>Module 3: FastAPI<br/><i>[CRUD APIs]</i><br/>Same UI actions, real persist"]
        U3["<b>Upcoming Module</b><br/>Module 4: LLM APIs<br/><i>[Streaming UI]</i><br/>Append messages at runtime"]
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
