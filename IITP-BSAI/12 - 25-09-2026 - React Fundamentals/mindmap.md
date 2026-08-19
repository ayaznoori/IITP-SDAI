```mermaid
flowchart TB
    subgraph foundation ["Foundation"]
        direction TB
        P1["<b>Previous Module</b><br/>Module 2: Web Fundamentals<br/><i>[HTML · CSS · DOM]</i><br/>Static + interactive pages"]
        CM["<b>Current Module Until Previous Session</b><br/>Module 4: AI Coding Partner<br/><i>[Cursor · Hygiene]</i><br/>Verified AI-assisted workflow"]
    end

    subgraph current ["Current Session"]
        CS["<b>Current Session</b><br/>React Fundamentals<br/><i>Mental shift:</i> from <b>page files</b> to <b>component trees</b><br/>Vite · JSX · props · lists"]
    end

    subgraph value ["Course & Real-Life Value"]
        CV["<b>Course Value</b><br/>Modern frontend standard<br/>Bridge to state, routing, full-stack"]
        RL["<b>Real-Life Use</b><br/>Dashboards · Social feeds · E-commerce UIs at scale"]
    end

    subgraph future ["Upcoming Modules"]
        direction TB
        U1["<b>Upcoming Module</b><br/>Module 5: React<br/><i>[useState · Forms]</i><br/>Interactive state-driven UI"]
        U2["<b>Upcoming Module</b><br/>Module 5: React<br/><i>[Fetch · Router]</i><br/>Multi-page apps"]
        U3["<b>Upcoming Module</b><br/>Module 6: Backend FastAPI<br/><i>[APIs · CORS]</i><br/>React + API integration"]
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
