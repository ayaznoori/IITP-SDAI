```mermaid
flowchart TB
    subgraph foundation ["Foundation"]
        direction TB
        P1["<b>Previous Module</b><br/>Module 10: Software 3.0<br/><i>[Prompts · LLM APIs]</i><br/>Models wrap text, not your files"]
        P2["<b>Previous Module</b><br/>Module 7: Database<br/><i>[SQL · ORM]</i><br/>Structured truth still lives in tables"]
        CM["<b>Current Module Until Previous Session</b><br/>Module 11: Industry Spotlight<br/><i>[Specs · Portfolio]</i><br/>You can define done and show work"]
    end

    subgraph current ["Current Session"]
        CS["<b>Current Session</b><br/>Masterclass: RAG Pipeline<br/><i>Mental shift:</i> from <b>ask the model</b> to <b>open-book retrieve then answer</b><br/>Embeddings idea · pipeline · quality check"]
    end

    subgraph value ["Course & Real-Life Value"]
        CV["<b>Course Value</b><br/>Choose RAG vs SQL vs plain LLM"]
        RL["<b>Real-Life Use</b><br/>Handbooks · support · private Q and A"]
    end

    subgraph future ["Upcoming Modules"]
        direction TB
        U1["<b>Upcoming Module</b><br/>Module 12: Capstone<br/><i>[Pages · APIs]</i><br/>Build the product slice"]
        U2["<b>Upcoming Module</b><br/>Module 12: Capstone<br/><i>[Integrate · Deploy]</i><br/>Public URL and demo"]
        U3["<b>Upcoming Module</b><br/>Programme close<br/><i>[Evaluation]</i><br/>Defend stack and judgement"]
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
