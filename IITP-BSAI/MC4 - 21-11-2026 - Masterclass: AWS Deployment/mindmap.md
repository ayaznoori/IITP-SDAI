```mermaid
flowchart TB
    subgraph foundation ["Foundation"]
        direction TB
        P1["<b>Previous Module</b><br/>Module 6: Backend FastAPI<br/><i>[API · CORS]</i><br/>What you actually ship"]
        P2["<b>Previous Module</b><br/>Module 7: Database<br/><i>[Neon · ORM]</i><br/>Data lives outside the box"]
        CM["<b>Current Module Until Previous Session</b><br/>Module 9: Deploy Ops<br/><i>[Docker · Actions]</i><br/>Image, CI, PaaS URL"]
    end

    subgraph current ["Current Session"]
        CS["<b>Current Session</b><br/>Masterclass: AWS Deployment<br/><i>Mental shift:</i> from <b>one PaaS button</b> to <b>a cloud map</b><br/>Outline · secrets · checklist"]
    end

    subgraph value ["Course & Real-Life Value"]
        CV["<b>Course Value</b><br/>Honest production literacy"]
        RL["<b>Real-Life Use</b><br/>Interviews · safer deploys · later scale"]
    end

    subgraph future ["Upcoming Modules"]
        direction TB
        U1["<b>Upcoming Module</b><br/>Module 10: Software 3.0<br/><i>[Prompts · LLM APIs]</i><br/>AI features on shipped backends"]
        U2["<b>Upcoming Module</b><br/>Module 11: Industry Spotlight<br/><i>[Specs · RAG]</i><br/>Product thinking plus retrieval"]
        U3["<b>Upcoming Module</b><br/>Module 12: Capstone<br/><i>[Build · Deploy]</i><br/>Public product story"]
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
