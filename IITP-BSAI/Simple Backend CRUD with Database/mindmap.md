```mermaid
flowchart TB
    subgraph foundation ["Foundation"]
        direction TB
        P1["<b>Previous Module</b><br/>Module 6: Backend FastAPI<br/><i>[CRUD · CORS]</i><br/>HTTP verbs in memory"]
        P2["<b>Previous Module</b><br/>Module 7 until ORM<br/><i>[Neon · SQLAlchemy]</i><br/>One model and GET read"]
        CM["<b>Current Module Until Previous Session</b><br/>Module 7: Database<br/><i>[SQL · ORM]</i><br/>Schema plus one read"]
    end

    subgraph current ["Current Session"]
        CS["<b>Current Session</b><br/>Simple Backend CRUD with Database<br/><i>Mental shift:</i> from <b>RAM lists</b> to <b>committed rows</b><br/>Session · CREATE/READ · one mutation"]
    end

    subgraph value ["Course & Real-Life Value"]
        CV["<b>Course Value</b><br/>API ready for tests and React"]
        RL["<b>Real-Life Use</b><br/>Tickets, notes, and carts that survive deploy"]
    end

    subgraph future ["Upcoming Modules"]
        direction TB
        U1["<b>Upcoming Module</b><br/>Module 8: Testing Hygiene<br/><i>[Pytest · CRUD lab]</i><br/>Checks and full-stack lab"]
        U2["<b>Upcoming Module</b><br/>Module 9: Deploy Ops<br/><i>[Docker · CI]</i><br/>Run the API anywhere"]
        U3["<b>Upcoming Module</b><br/>Module 10: Software 3.0<br/><i>[LLM APIs]</i><br/>AI on a real backend"]
    end

    P1 ==>|&nbsp;Foundation&nbsp;| P2
    P2 ==>|&nbsp;Builds on&nbsp;| CM
    CM ==>|&nbsp;Blueprint&nbsp;| CS
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
