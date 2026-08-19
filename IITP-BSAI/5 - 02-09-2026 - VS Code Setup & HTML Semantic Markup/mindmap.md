```mermaid
flowchart TB
    subgraph foundation ["Foundation"]
        direction TB
        P1["<b>Previous Module</b><br/>Module 1: Programming Foundations<br/><i>[JavaScript · Core Syntax]</i><br/>Variables · loops · arrays · functions"]
        CM["<b>Current Module Until Previous Session</b><br/>Module 1 complete · Module 2 starting<br/><i>[JS in One Compiler]</i><br/>All foundational JS sessions done"]
    end

    subgraph current ["Current Session"]
        CS["<b>Current Session</b><br/>VS Code Setup & HTML Semantic Markup<br/><i>Mental shift:</i> from <b>console programs</b> to <b>web documents</b><br/>Local editor · HTML skeleton · semantics · forms · DevTools"]
    end

    subgraph value ["Course & Real-Life Value"]
        CV["<b>Course Value</b><br/>Gateway to CSS, DOM, React, and full-stack UI<br/>Every visible screen starts with HTML"]
        RL["<b>Real-Life Use</b><br/>Landing pages · Login forms · Portfolio sites · Accessible product markup"]
    end

    subgraph future ["Upcoming Modules"]
        direction TB
        U1["<b>Upcoming Module</b><br/>Module 2: Web Fundamentals<br/><i>[CSS · Layout]</i><br/>Flexbox · Grid · responsive design"]
        U2["<b>Upcoming Module</b><br/>Module 3: Version Control<br/><i>[Git · GitHub]</i><br/>Track and share project files"]
        U3["<b>Upcoming Module</b><br/>Module 5: React<br/><i>[Components · JSX]</i><br/>HTML-like syntax in components"]
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
