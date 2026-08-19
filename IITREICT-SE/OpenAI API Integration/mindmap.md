```mermaid
%%{init: {'flowchart': {'nodeSpacing': 42, 'rankSpacing': 54, 'padding': 18}}}%%
flowchart TB
    subgraph foundation ["Foundation"]
        direction TB
        P1["<b>Previous Module</b><br/>Module 1: Developer Setup<br/><i>[venv · env files]</i><br/>Secrets stay out of git"]
        P2["<b>Previous Module</b><br/>Module 3: FastAPI Backend<br/><i>[JSON · errors]</i><br/>HTTP clients and contracts"]
        CM["<b>Current Module Until Previous Session</b><br/>Module 4: LLM Foundations<br/><i>[Prompts · Knobs]</i><br/>Roles · temperature · max tokens · stop"]
    end

    subgraph current ["Current Session"]
        CS["<b>Current Session</b><br/>OpenAI API Integration<br/><i>Mental shift:</i> from <b>playground typing</b> to <b>Python product calls</b><br/>Auth · Chat Completions · response shape · retries"]
    end

    subgraph value ["Course and Real-Life Value"]
        CV["<b>Course Value</b><br/>Client helper you will mount on FastAPI"]
        RL["<b>Real-Life Use</b><br/>Server-side keys · usage logs · calm 429 handling"]
    end

    subgraph future ["Upcoming Modules"]
        direction TB
        U1["<b>Upcoming Module</b><br/>Module 5: AI-First Development<br/><i>[Copilot · Specs]</i><br/>Build features faster"]
        U2["<b>Upcoming Module</b><br/>Module 6: Shipping AI Apps<br/><i>[Deploy · LLMOps]</i><br/>Keys in production config"]
        U3["<b>Upcoming Module</b><br/>Module 7: Capstone Product<br/><i>[Python · OpenAI]</i><br/>Live model in the portfolio app"]
    end

    P1 ==>|&nbsp;Foundation&nbsp;| P2
    P2 ==>|&nbsp;HTTP Skills&nbsp;| CM
    CM ==>|&nbsp;Builds On&nbsp;| CS
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
