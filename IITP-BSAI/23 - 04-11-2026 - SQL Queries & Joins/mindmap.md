```mermaid
%%{init: {'flowchart': {'nodeSpacing': 42, 'rankSpacing': 52, 'diagramPadding': 16}}}%%
flowchart TB
    subgraph foundation ["Foundation"]
        direction TB
        P1["<b>Previous Module</b><br/>Module 6: Backend FastAPI<br/><i>[CRUD · Lab]</i><br/>API shape without SQL"]
        P2["<b>Previous Module</b><br/>Module 5: Frontend React<br/><i>[Fetch · Lists]</i><br/>UI will display query results"]
        CM["<b>Current Module Until Previous Session</b><br/>Module 7: Database<br/><i>[Neon · CREATE TABLE]</i><br/>Empty tables ready to query"]
    end

    subgraph current ["Current Session"]
        CS["<b>Current Session</b><br/>SQL Queries and Joins<br/><i>Mental shift:</i> from <b>Python lists</b> to <b>asking Postgres</b><br/>CRUD SQL · WHERE · FK · INNER JOIN"]
    end

    subgraph value ["Course & Real-Life Value"]
        CV["<b>Course Value</b><br/>Required before ORM mapping<br/>Read logs and dashboards"]
        RL["<b>Real-Life Use</b><br/>Reports · support queries · interviews"]
    end

    subgraph future ["Upcoming Modules"]
        direction TB
        U1["<b>Upcoming Module</b><br/>Module 7: Database next<br/><i>[ORM · Schema]</i><br/>SQLAlchemy talks to Neon"]
        U2["<b>Upcoming Module</b><br/>Module 8: Testing<br/><i>[Pytest · Review]</i><br/>Tests on persistent CRUD"]
        U3["<b>Upcoming Module</b><br/>Module 9: Deployment<br/><i>[Docker · CI]</i><br/>Then AI APIs in Module 10"]
    end

    P1 ==>|&nbsp;Foundation&nbsp;| P2
    P2 ==>|&nbsp;Tables Ready&nbsp;| CM
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
