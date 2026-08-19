```mermaid
flowchart TB
    subgraph foundation ["Foundation"]
        direction TB
        P1["<b>Previous Module</b><br/>Module 1: Programming Foundations<br/><i>[JavaScript · Loops]</i><br/>for/while · patterns · repetition"]
        CM["<b>Current Module Until Previous Session</b><br/>Module 1: Programming Foundations<br/><i>[JavaScript · Data]</i><br/>Arrays · strings · objects · map"]
    end

    subgraph current ["Current Session"]
        CS["<b>Current Session</b><br/>Functions in JavaScript<br/><i>Mental shift:</i> from <b>inline steps</b> to <b>reusable units</b><br/>Parameters · return · scope · arrow functions · refactor"]
    end

    subgraph value ["Course & Real-Life Value"]
        CV["<b>Course Value</b><br/>Prepares for React components, utilities, and clean modules<br/>Ends Module 1 JS foundations before web stack"]
        RL["<b>Real-Life Use</b><br/>Shared validators · Pricing helpers · Auth checks · Testable business logic"]
    end

    subgraph future ["Upcoming Modules"]
        direction TB
        U1["<b>Upcoming Module</b><br/>Module 2: Web Fundamentals<br/><i>[HTML · CSS · DOM]</i><br/>Structure pages · style · events"]
        U2["<b>Upcoming Module</b><br/>Module 5: React<br/><i>[Components · Props]</i><br/>Functions as UI building blocks"]
        U3["<b>Upcoming Module</b><br/>Module 6: Backend FastAPI<br/><i>[Python · APIs]</i><br/>Route handlers as functions"]
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
