```mermaid
flowchart TB
    subgraph foundation ["Foundation"]
        direction TB
        P1["<b>Previous Module</b><br/>Module 1: Developer Setup<br/><i>[Python · Git]</i><br/>Local dev · version control · PRs"]
        CM["<b>Current Module Until Previous Session</b><br/>Module 1 complete<br/><i>[Evaluation ready]</i><br/>Python · projects · Git collaboration"]
    end

    subgraph current ["Current Session"]
        CS["<b>Current Session</b><br/>HTML & Semantic Markup<br/><i>Mental shift:</i> from <b>backend logic</b> to <b>user-facing structure</b><br/>Semantics · forms · accessibility · DevTools"]
    end

    subgraph value ["Course & Real-Life Value"]
        CV["<b>Course Value</b><br/>Starts Module 2 web stack<br/>Foundation for CSS, JS, React, and API UIs"]
        RL["<b>Real-Life Use</b><br/>Portfolio pages · Login forms · Product landing structure"]
    end

    subgraph future ["Upcoming Modules"]
        direction TB
        U1["<b>Upcoming Module</b><br/>Module 2: Web Fundamentals<br/><i>[CSS · Layout]</i><br/>Styling and responsive design"]
        U2["<b>Upcoming Module</b><br/>Module 2: Web Fundamentals<br/><i>[JavaScript · DOM]</i><br/>Browser interactivity"]
        U3["<b>Upcoming Module</b><br/>Module 3: FastAPI<br/><i>[REST · APIs]</i><br/>Connect frontend to backend"]
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
