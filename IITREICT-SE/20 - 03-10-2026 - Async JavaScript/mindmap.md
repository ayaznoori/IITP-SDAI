```mermaid
flowchart TB
    subgraph foundation ["Foundation"]
        P1["<b>Previous Module</b><br/>Module 2: Web Fundamentals<br/><i>[HTML · CSS]</i><br/>Pages users can see"]
        P2["<b>Previous Module</b><br/>Module 2: Web Fundamentals<br/><i>[JS Fundamentals]</i><br/>Functions · control flow"]
        CM["<b>Current Module Until Previous Session</b><br/>Module 2: Web Fundamentals<br/><i>[Modern JS · Web eras]</i><br/>ES6+ · why the web evolved"]
    end
    subgraph current ["Current Session"]
        CS["<b>Current Session</b><br/>Async JavaScript<br/><i>Mental shift:</i> <b>blocking lines</b> → <b>scheduled work</b><br/>callbacks · Promises · async/await"]
    end
    subgraph value ["Course & Real-Life Value"]
        CV["<b>Course Value</b><br/>Required before Fetch and React effects"]
        RL["<b>Real-Life Use</b><br/>OTP waits · Gmail load · checkout steps"]
    end
    subgraph future ["Upcoming Modules"]
        U1["<b>Upcoming Module</b><br/>Module 2: Web Fundamentals<br/><i>[DOM · HTTP · React]</i><br/>Events then remote JSON"]
        U2["<b>Upcoming Module</b><br/>Module 3: FastAPI<br/><i>[REST · JSON]</i><br/>Server waits you already modeled"]
        U3["<b>Upcoming Module</b><br/>Module 4: LLM APIs<br/><i>[Async HTTP]</i><br/>Long model calls"]
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
