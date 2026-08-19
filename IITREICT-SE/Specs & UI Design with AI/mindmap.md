```mermaid
%%{init: {'flowchart': {'nodeSpacing': 42, 'rankSpacing': 54, 'padding': 18}}}%%
flowchart TB
    subgraph foundation ["Foundation"]
        direction TB
        P1["<b>Previous Module</b><br/>Module 2: Web Fundamentals<br/><i>[REST · UI]</i><br/>APIs as contracts"]
        P2["<b>Previous Module</b><br/>Module 4: LLM APIs<br/><i>[FastAPI · JSON]</i><br/>Classify endpoint exists"]
        CM["<b>Current Module Until Previous Session</b><br/>Module 5: AI-First Development<br/><i>[Copilot · Context]</i><br/>Grounded coding help"]
    end

    subgraph current ["Current Session"]
        CS["<b>Current Session</b><br/>Specs and UI Design with AI<br/><i>Mental shift:</i> from <b>jumping to code</b> to <b>testable product briefs</b><br/>PRD · stories · contracts · flows · human cuts"]
    end

    subgraph value ["Course and Real-Life Value"]
        CV["<b>Course Value</b><br/>Specs agents and capstone will follow"]
        RL["<b>Real-Life Use</b><br/>Jira tickets · Figma journeys · edge cases"]
    end

    subgraph future ["Upcoming Modules"]
        direction TB
        U1["<b>Upcoming Module</b><br/>Module 6: Shipping AI Apps<br/><i>[Quality · Tests]</i><br/>AC become test ideas"]
        U2["<b>Upcoming Module</b><br/>Module 7: Capstone Product<br/><i>[PRD · Build]</i><br/>You already practiced the spec"]
    end

    P1 ==>|&nbsp;Foundation&nbsp;| P2
    P2 ==>|&nbsp;Feature Exists&nbsp;| CM
    CM ==>|&nbsp;Builds On&nbsp;| CS
    CS ==>|&nbsp;Course Path&nbsp;| CV
    CS ==>|&nbsp;Real-Life Use&nbsp;| RL
    CS ==>|&nbsp;Next Module&nbsp;| U1
    U1 -.-> U2

    classDef previous fill:#E8F4FD,stroke:#4A90D9,stroke-width:2px,color:#1a1a1a
    classDef current fill:#FFF3CD,stroke:#E6A817,stroke-width:3px,color:#1a1a1a
    classDef value fill:#D4EDDA,stroke:#28A745,stroke-width:2px,color:#1a1a1a
    classDef future fill:#F3E8FF,stroke:#9B59B6,stroke-width:2px,color:#1a1a1a

    class P1,P2,CM previous
    class CS current
    class CV,RL value
    class U1,U2 future

    linkStyle default stroke-width:3px
```
