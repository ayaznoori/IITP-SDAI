```mermaid
flowchart TB
    subgraph foundation ["Foundation"]
        direction TB
        P1["<b>Previous Module</b><br/>JS Foundations<br/><i>[Functions · Data]</i><br/>Logic you can explain out loud"]
        P2["<b>Previous Module</b><br/>Web Fundamentals<br/><i>[HTML · CSS]</i><br/>Layout and page structure"]
        CM["<b>Current Module Until Previous Session</b><br/>Frontend React<br/><i>[State · Tailwind · Fetch · Router]</i><br/>Ingredients for a small UI"]
    end

    subgraph current ["Current Session"]
        CS["<b>Current Session</b><br/>Hands-on React Build Lab<br/><i>Mental shift:</i> from <b>watching demos</b> to <b>a working mini screen</b><br/>Components · state · Tailwind · debug"]
    end

    subgraph value ["Course & Real-Life Value"]
        CV["<b>Course Value</b><br/>Prove React skills by building<br/>Prep for deploy session"]
        RL["<b>Real-Life Use</b><br/>Intern tickets · Widget-sized product UI"]
    end

    subgraph future ["Upcoming Modules"]
        direction TB
        U1["<b>Upcoming Module</b><br/>Python & FastAPI<br/><i>[APIs · CORS]</i><br/>Wire the UI to your API"]
        U2["<b>Upcoming Module</b><br/>Databases<br/><i>[SQL · Postgres]</i><br/>Save what users change"]
        U3["<b>Upcoming Module</b><br/>Deploy & AI<br/><i>[Cloud · LLM APIs]</i><br/>Live apps and AI features"]
    end

    P1 ==>|&nbsp;Foundation&nbsp;| P2
    P2 ==>|&nbsp;Builds on&nbsp;| CM
    CM ==>|&nbsp;Components&nbsp;| CS
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
