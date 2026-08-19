```mermaid
flowchart TB
    subgraph foundation ["Foundation"]
        direction TB
        P1["<b>Previous Module</b><br/>Module 1: Developer Setup<br/><i>[Python · try/except]</i><br/>Errors are not atomic DB writes"]
        P2["<b>Previous Module</b><br/>Module 2: Web Fundamentals<br/><i>[HTTP · APIs]</i><br/>One request can mean many writes"]
        CM["<b>Current Module Until Previous Session</b><br/>Module 3: FastAPI Backend<br/><i>[SQL I · SQL II]</i><br/>SELECT INSERT UPDATE DELETE WHERE"]
    end

    subgraph current ["Current Session"]
        CS["<b>Current Session</b><br/>Masterclass: Transactions and ACID<br/><i>Mental shift:</i> from <b>single statements</b> to <b>units of work</b><br/>Commit · rollback · ACID why"]
    end

    subgraph value ["Course & Real-Life Value"]
        CV["<b>Course Value</b><br/>Makes ORM commit meaningful<br/>Safer capstone money and seat flows"]
        RL["<b>Real-Life Use</b><br/>UPI · ticketing · inventory · signup pairs"]
    end

    subgraph future ["Upcoming Modules"]
        direction TB
        U1["<b>Upcoming Module</b><br/>Module 4: LLM & OpenAI APIs<br/><i>[API calls]</i><br/>Do not confuse LLM retry with DB rollback"]
        U2["<b>Upcoming Module</b><br/>Module 6: Shipping AI Apps<br/><i>[Prod quality]</i><br/>Durable records of AI usage"]
        U3["<b>Upcoming Module</b><br/>Module 7: Capstone<br/><i>[Backend · DB]</i><br/>Transactional product writes"]
    end

    P1 ==>|&nbsp;Foundation&nbsp;| P2
    P2 ==>|&nbsp;Request Scope&nbsp;| CM
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
