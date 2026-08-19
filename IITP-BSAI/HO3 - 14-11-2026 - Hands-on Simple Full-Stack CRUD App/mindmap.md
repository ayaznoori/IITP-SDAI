```mermaid
flowchart TB
    subgraph foundation ["Foundation"]
        direction TB
        P1["<b>Previous Module</b><br/>Module 5: React Frontend<br/><i>[State · Fetch]</i><br/>UI and HTTP from the browser"]
        P2["<b>Previous Module</b><br/>Module 7: Database<br/><i>[ORM · CRUD]</i><br/>Persistent FastAPI"]
        CM["<b>Current Module Until Previous Session</b><br/>Module 8: Testing Hygiene<br/><i>[Pytest]</i><br/>GET and POST checks"]
    end

    subgraph current ["Current Session"]
        CS["<b>Current Session</b><br/>Hands-on Simple Full-Stack CRUD App<br/><i>Mental shift:</i> from <b>two demos</b> to <b>one synced product</b><br/>Schema · CRUD UI · fetch"]
    end

    subgraph value ["Course & Real-Life Value"]
        CV["<b>Course Value</b><br/>End-to-end resource before deploy"]
        RL["<b>Real-Life Use</b><br/>Issue trackers · notes · admin tables"]
    end

    subgraph future ["Upcoming Modules"]
        direction TB
        U1["<b>Upcoming Module</b><br/>Module 9: Deploy Ops<br/><i>[Docker · Cloud]</i><br/>Ship the backend"]
        U2["<b>Upcoming Module</b><br/>Module 10: Software 3.0<br/><i>[LLM APIs]</i><br/>Add AI on the same stack"]
        U3["<b>Upcoming Module</b><br/>Module 11: Industry Spotlight<br/><i>[Specs · RAG]</i><br/>Product and AI depth"]
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
