```mermaid
flowchart TB
    subgraph foundation ["Foundation"]
        direction TB
        P1["<b>Previous Module</b><br/>Module 2: Web Fundamentals<br/><i>[HTTP · Cookies intro]</i><br/>Clients send credentials"]
        P2["<b>Previous Module</b><br/>Module 1: Developer Setup<br/><i>[Python · secrets mindset]</i><br/>Do not hardcode blindly"]
        CM["<b>Current Module Until Previous Session</b><br/>Module 3: FastAPI Backend<br/><i>[ORM · Depends]</i><br/>Routes and reusable gates"]
    end

    subgraph current ["Current Session"]
        CS["<b>Current Session</b><br/>Authentication with JWT<br/><i>Mental shift:</i> from <b>open APIs</b> to <b>proven identity</b><br/>bcrypt · login · expiring JWT"]
    end

    subgraph value ["Course & Real-Life Value"]
        CV["<b>Course Value</b><br/>Token for protected routes next<br/>Capstone login flow"]
        RL["<b>Real-Life Use</b><br/>SPA login · mobile APIs · session-less auth"]
    end

    subgraph future ["Upcoming Modules"]
        direction TB
        U1["<b>Upcoming Module</b><br/>Module 4: LLM & OpenAI APIs<br/><i>[API keys]</i><br/>Different secret; same care"]
        U2["<b>Upcoming Module</b><br/>Module 6: Shipping AI Apps<br/><i>[Secrets · Deploy]</i><br/>JWT secret in env"]
        U3["<b>Upcoming Module</b><br/>Module 7: Capstone<br/><i>[Auth]</i><br/>Login in the product"]
    end

    P1 ==>|&nbsp;Foundation&nbsp;| P2
    P2 ==>|&nbsp;Careful Secrets&nbsp;| CM
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
