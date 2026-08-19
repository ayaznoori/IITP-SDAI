```mermaid
flowchart TB
    subgraph foundation ["Foundation"]
        direction TB
        P1["<b>Previous Module</b><br/>Module 4: AI Coding Partner<br/><i>[Review · Small diffs]</i><br/>Hygiene you must revive"]
        P2["<b>Previous Module</b><br/>Module 9: Deploy Ops<br/><i>[Live URL · CI]</i><br/>Something public to defend"]
        CM["<b>Current Module Until Previous Session</b><br/>Module 11: Industry Spotlight<br/><i>[Specs]</i><br/>You know what done means"]
    end

    subgraph current ["Current Session"]
        CS["<b>Current Session</b><br/>Human-AI Workflows & Portfolio Readiness<br/><i>Mental shift:</i> from <b>generated code</b> to <b>owned evidence</b><br/>Verify · README · hiring pitch"]
    end

    subgraph value ["Course & Real-Life Value"]
        CV["<b>Course Value</b><br/>Capstone starts from a clean story"]
        RL["<b>Real-Life Use</b><br/>Interviews · internships · trust"]
    end

    subgraph future ["Upcoming Modules"]
        direction TB
        U1["<b>Upcoming Module</b><br/>Module 11 continues<br/><i>[RAG ideas]</i><br/>Why private context needs retrieval"]
        U2["<b>Upcoming Module</b><br/>Module 12: Capstone<br/><i>[FE · BE build]</i><br/>Core screens and APIs"]
        U3["<b>Upcoming Module</b><br/>Module 12: Capstone<br/><i>[Deploy · Demo]</i><br/>Public URL and quality gate"]
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
