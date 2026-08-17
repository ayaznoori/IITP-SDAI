```mermaid
flowchart TB
    subgraph foundation ["Foundation"]
        direction TB
        P1["<b>Previous Module</b><br/>Module 1: Programming Foundations<br/><i>[Python · Control Flow]</i><br/>Loops · functions · data structures"]
        CM["<b>Current Module Until Previous Session</b><br/>Module 1: Programming Foundations<br/><i>[Python · Algorithms]</i><br/>Bubble/selection sort · linear/binary search"]
    end

    subgraph current ["Current Session"]
        CS["<b>Current Session</b><br/>Hands-on DSA Problem Solving<br/><i>Mental shift:</i> from <b>learning tools</b> to <b>solving problems</b><br/>Decompose · combine skills · nested logic · guided practice"]
    end

    subgraph value ["Course & Real-Life Value"]
        CV["<b>Course Value</b><br/>Caps Module 1 Python/DSA before local dev setup<br/>Interview-style thinking for the full programme"]
        RL["<b>Real-Life Use</b><br/>Debug multi-step bugs · Design small utilities · Reason about data pipelines"]
    end

    subgraph future ["Upcoming Modules"]
        direction TB
        U1["<b>Upcoming Module</b><br/>Module 1: Developer Setup<br/><i>[VS Code · Modules · venv]</i><br/>Local Python · project structure"]
        U2["<b>Upcoming Module</b><br/>Module 2: Web Fundamentals<br/><i>[HTML · CSS · JavaScript]</i><br/>Frontend pages and styling"]
        U3["<b>Upcoming Module</b><br/>Module 3: FastAPI Backend<br/><i>[REST · Pydantic]</i><br/>API development"]
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
