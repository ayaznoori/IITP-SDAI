```mermaid
%%{init: {'flowchart': {'nodeSpacing': 42, 'rankSpacing': 52, 'diagramPadding': 16}}}%%
flowchart TB
    subgraph foundation ["Foundation"]
        direction TB
        P1["<b>Previous Module</b><br/>Module 5: Frontend React<br/><i>[Fetch · CRUD UI]</i><br/>Users expect saved data"]
        P2["<b>Previous Module</b><br/>Module 6: Backend FastAPI<br/><i>[CRUD · CORS]</i><br/>In-memory API works until restart"]
        CM["<b>Current Module Until Previous Session</b><br/>Module 6 complete for RAM APIs<br/><i>[POST · React]</i><br/>Full-stack loop without disk"]
    end

    subgraph current ["Current Session"]
        CS["<b>Current Session</b><br/>PostgreSQL and Neon Setup<br/><i>Mental shift:</i> from <b>RAM lists</b> to <b>persistent tables</b><br/>project · CREATE TABLE · primary key"]
    end

    subgraph value ["Course & Real-Life Value"]
        CV["<b>Course Value</b><br/>Foundation for SQL and ORM<br/>Shared cloud database for labs"]
        RL["<b>Real-Life Use</b><br/>Orders · users · grades that survive deploys"]
    end

    subgraph future ["Upcoming Modules"]
        direction TB
        U1["<b>Upcoming Module</b><br/>Module 7: Database<br/><i>[SQL · ORM]</i><br/>Query and connect FastAPI"]
        U2["<b>Upcoming Module</b><br/>Module 8: Testing<br/><i>[Pytest · Review]</i><br/>Test persistent routes later"]
        U3["<b>Upcoming Module</b><br/>Module 9: Deployment<br/><i>[Docker · CI]</i><br/>Env vars for database URLs"]
    end

    P1 ==>|&nbsp;Foundation&nbsp;| P2
    P2 ==>|&nbsp;Need Persist&nbsp;| CM
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
