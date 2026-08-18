```mermaid
flowchart TB
    subgraph foundation ["Foundation"]
        direction TB
        P1["<b>Previous Module</b><br/>Module 1: Python Foundations<br/><i>[Python · Projects]</i><br/>Code · venv · JSON utilities"]
        CM["<b>Current Module Until Previous Session</b><br/>Module 1: Developer Setup<br/><i>[Git · Local]</i><br/>init · commit · push · clone · README"]
    end

    subgraph current ["Current Session"]
        CS["<b>Current Session</b><br/>Git Collaboration, Branching & PRs<br/><i>Mental shift:</i> from <b>solo commits</b> to <b>team workflow</b><br/>Branches · merge · conflicts · review"]
    end

    subgraph value ["Course & Real-Life Value"]
        CV["<b>Course Value</b><br/>Industry-standard collaboration before web modules<br/>Prepares for group capstone work"]
        RL["<b>Real-Life Use</b><br/>Feature branches · Code review · Safe releases"]
    end

    subgraph future ["Upcoming Modules"]
        direction TB
        U1["<b>Upcoming Module</b><br/>Module 2: Web Fundamentals<br/><i>[HTML · CSS]</i><br/>Frontend pages in Git repos"]
        U2["<b>Upcoming Module</b><br/>Module 3: FastAPI<br/><i>[REST · Backend]</i><br/>API repos and deployment"]
        U3["<b>Upcoming Module</b><br/>Module 7: Capstone<br/><i>[Full Stack · AI]</i><br/>Team delivery on GitHub"]
    end

    P1 ==>|&nbsp;Foundation&nbsp;| CM
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

    class P1,CM previous
    class CS current
    class CV,RL value
    class U1,U2,U3 future

    linkStyle default stroke-width:3px
```
