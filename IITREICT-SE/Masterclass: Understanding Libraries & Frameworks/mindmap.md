```mermaid
flowchart TB
    subgraph foundation ["Foundation"]
        P1["<b>Previous Module</b><br/>Module 2: Web Fundamentals<br/><i>[Vanilla JS]</i><br/>You can build UI by hand"]
        P2["<b>Previous Module</b><br/>Module 2: Web Fundamentals<br/><i>[Fetch · DOM]</i><br/>JSON into lists"]
        CM["<b>Current Module Until Previous Session</b><br/>Module 2: Web Fundamentals<br/><i>[REST in the browser]</i><br/>Working todos page without React"]
    end
    subgraph current ["Current Session"]
        CS["<b>Current Session</b><br/>Masterclass: Libraries and Frameworks<br/><i>Mental shift:</i> <b>heroic from-scratch</b> → <b>chosen leverage</b><br/>library vs framework · why React-like UI"]
    end
    subgraph value ["Course & Real-Life Value"]
        CV["<b>Course Value</b><br/>Justifies the React weekend with judgment"]
        RL["<b>Real-Life Use</b><br/>Tool choice in internships and code reviews"]
    end
    subgraph future ["Upcoming Modules"]
        U1["<b>Upcoming Module</b><br/>Module 2: Web Fundamentals<br/><i>[React]</i><br/>Components · hooks · CRUD"]
        U2["<b>Upcoming Module</b><br/>Module 3: FastAPI<br/><i>[Backend framework]</i><br/>Same idea on the server"]
        U3["<b>Upcoming Module</b><br/>Module 4: LLM APIs<br/><i>[SDK libraries]</i><br/>Call models, do not train them"]
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
