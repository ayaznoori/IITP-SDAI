```mermaid
flowchart TB
    subgraph foundation ["Foundation"]
        direction TB
        P1["<b>Previous Module</b><br/>Module 1: Programming Foundations<br/><i>[JavaScript]</i><br/>Logic · data · functions"]
        CM["<b>Current Module Until Previous Session</b><br/>Module 2: Web Fundamentals<br/><i>[HTML · CSS Grid]</i><br/>Structure · styling · responsive layout"]
    end

    subgraph current ["Current Session"]
        CS["<b>Current Session</b><br/>DOM Manipulation & Event Handling<br/><i>Mental shift:</i> from <b>static pages</b> to <b>live interactive UI</b><br/>DOM tree · selectors · events · micro-apps"]
    end

    subgraph value ["Course & Real-Life Value"]
        CV["<b>Course Value</b><br/>Closes Module 2 web trilogy<br/>Direct bridge to React state and effects"]
        RL["<b>Real-Life Use</b><br/>Form validation · Toggles · Live search · Cart updates"]
    end

    subgraph future ["Upcoming Modules"]
        direction TB
        U1["<b>Upcoming Module</b><br/>Module 3: Version Control<br/><i>[Git · GitHub]</i><br/>Collaborate on web projects"]
        U2["<b>Upcoming Module</b><br/>Module 5: React<br/><i>[Components · Hooks]</i><br/>Declarative UI over the DOM"]
        U3["<b>Upcoming Module</b><br/>Module 6: FastAPI Backend<br/><i>[Python · APIs]</i><br/>Full-stack integration"]
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
