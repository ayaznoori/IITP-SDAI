```mermaid
flowchart TB
    subgraph foundation ["Foundation"]
        P1["<b>Previous Module</b><br/>Module 1: Developer Setup<br/><i>[Python · Git]</i><br/>Local programs and repos"]
        P2["<b>Previous Module</b><br/>Module 2: Web Fundamentals<br/><i>[HTML · JS DOM]</i><br/>What the tab can do alone"]
        CM["<b>Current Module Until Previous Session</b><br/>Module 2: Web Fundamentals<br/><i>[Runtime DOM]</i><br/>UI without explaining the wire"]
    end
    subgraph current ["Current Session"]
        CS["<b>Current Session</b><br/>How the Web Works<br/><i>Mental shift:</i> <b>a file in a folder</b> → <b>request and response</b><br/>HTTP · cookies · CORS · URL trace"]
    end
    subgraph value ["Course & Real-Life Value"]
        CV["<b>Course Value</b><br/>Network tab literacy before APIs"]
        RL["<b>Real-Life Use</b><br/>Debug 404 vs 401 vs CORS at work"]
    end
    subgraph future ["Upcoming Modules"]
        U1["<b>Upcoming Module</b><br/>Module 2: Web Fundamentals<br/><i>[REST · Fetch]</i><br/>JSON over the same HTTP"]
        U2["<b>Upcoming Module</b><br/>Module 3: FastAPI<br/><i>[Servers · CORS]</i><br/>You send the status codes"]
        U3["<b>Upcoming Module</b><br/>Module 4: LLM APIs<br/><i>[HTTPS clients]</i><br/>Same verbs, model hosts"]
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
