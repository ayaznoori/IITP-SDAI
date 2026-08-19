```mermaid
flowchart TB
    subgraph foundation ["Foundation"]
        direction TB
        P1["<b>Previous Module</b><br/>Module 6: Backend FastAPI<br/><i>[Uvicorn · Pydantic]</i><br/>In-memory JSON APIs"]
        P2["<b>Previous Module</b><br/>Module 5: React Frontend<br/><i>[Fetch · Router]</i><br/>UI that calls APIs"]
        CM["<b>Current Module Until Previous Session</b><br/>Module 7: Database<br/><i>[Neon · SQL]</i><br/>Tables, keys, and joins"]
    end

    subgraph current ["Current Session"]
        CS["<b>Current Session</b><br/>ORM, Neon Connection & Schema Design<br/><i>Mental shift:</i> from <b>SQL strings</b> to <b>Python models</b><br/>SQLAlchemy · URL · one GET read"]
    end

    subgraph value ["Course & Real-Life Value"]
        CV["<b>Course Value</b><br/>Persistent backend for full-stack CRUD"]
        RL["<b>Real-Life Use</b><br/>Hosted Postgres · ORM routes at startups"]
    end

    subgraph future ["Upcoming Modules"]
        direction TB
        U1["<b>Upcoming Module</b><br/>Module 8: Testing Hygiene<br/><i>[Pytest · Review]</i><br/>Automated API checks"]
        U2["<b>Upcoming Module</b><br/>Module 9: Deploy Ops<br/><i>[Docker · CI]</i><br/>Ship the API"]
        U3["<b>Upcoming Module</b><br/>Module 10: Software 3.0<br/><i>[LLM · Prompts]</i><br/>AI features on APIs"]
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
