```mermaid
flowchart TB
    subgraph foundation ["Foundation"]
        direction TB
        P1["<b>Previous Module</b><br/>Programme Onboarding<br/>(Virtual Inaugural)<br/><i>[Orientation · Mindset]</i><br/>Course goals · Learning path"]
        CM["<b>Current Module Until Previous Session</b><br/>Module 1: Programming Foundations<br/><i>[JavaScript · One Compiler]</i><br/>Starting fresh — no prior coding sessions"]
    end

    subgraph current ["Current Session"]
        CS["<b>Current Session</b><br/>Variables, Data Types, Operators & Conditionals<br/><i>Mental shift:</i> from reading code to <b>writing decisions</b><br/>Store data · Compare values · Branch with if/else"]
    end

    subgraph value ["Course & Real-Life Value"]
        CV["<b>Course Value</b><br/>First real JavaScript building blocks<br/>Feeds loops, arrays, functions, and React later"]
        RL["<b>Real-Life Use</b><br/>Login checks · Price comparisons · Form validation · Any app logic that says 'if this, do that'"]
    end

    subgraph future ["Upcoming Modules"]
        direction TB
        U1["<b>Upcoming Module</b><br/>Module 1: Programming Foundations<br/><i>[JavaScript · Control Flow]</i><br/>Loops · Arrays · Functions"]
        U2["<b>Upcoming Module</b><br/>Module 2: Web Fundamentals<br/><i>[HTML · CSS]</i><br/>Pages · Layout · Styling"]
        U3["<b>Upcoming Module</b><br/>Module 3: Version Control<br/><i>[Git · GitHub]</i><br/>Track changes · Collaborate"]
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
