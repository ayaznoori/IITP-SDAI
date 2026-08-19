```mermaid
flowchart TB
    subgraph foundation ["Foundation"]
        direction TB
        P1["<b>Previous Module</b><br/>Module 2: Web Fundamentals<br/><i>[CORS · Fetch · HTTP]</i><br/>Browser origin rules"]
        P2["<b>Previous Module</b><br/>Module 1: Developer Setup<br/><i>[Files · JSON I/O]</i><br/>open and write on disk"]
        CM["<b>Current Module Until Previous Session</b><br/>Module 3: FastAPI Backend<br/><i>[Middleware · Depends]</i><br/>Request wrap and shared logic"]
    end

    subgraph current ["Current Session"]
        CS["<b>Current Session</b><br/>File Handling and CORS<br/><i>Mental shift:</i> from <b>JSON-only APIs</b> to <b>browser + files</b><br/>CORS · upload · download · tasks"]
    end

    subgraph value ["Course & Real-Life Value"]
        CV["<b>Course Value</b><br/>React can call FastAPI locally<br/>Capstone resume or image upload"]
        RL["<b>Real-Life Use</b><br/>KYC docs · exports · after-response logging"]
    end

    subgraph future ["Upcoming Modules"]
        direction TB
        U1["<b>Upcoming Module</b><br/>Module 4: LLM & OpenAI APIs<br/><i>[Files · Prompts]</i><br/>Later multimodal inputs"]
        U2["<b>Upcoming Module</b><br/>Module 5: AI-First Development<br/><i>[Agents]</i><br/>Upload flows in products"]
        U3["<b>Upcoming Module</b><br/>Module 6: Shipping AI Apps<br/><i>[Multimodal · Deploy]</i><br/>Files in production APIs"]
    end

    P1 ==>|&nbsp;Foundation&nbsp;| P2
    P2 ==>|&nbsp;Disk I/O&nbsp;| CM
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
