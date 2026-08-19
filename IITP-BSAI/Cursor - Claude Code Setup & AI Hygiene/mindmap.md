```mermaid
flowchart TB
    subgraph foundation ["Foundation"]
        direction TB
        P1["<b>Previous Module</b><br/>Module 3: Version Control<br/><i>[Git · PRs]</i><br/>Branches · merge · review"]
        CM["<b>Current Module Until Previous Session</b><br/>Module 3 complete<br/><i>[GitHub workflow]</i><br/>Collaboration ready"]
    end

    subgraph current ["Current Session"]
        CS["<b>Current Session</b><br/>Cursor / Claude Code & AI Hygiene<br/><i>Mental shift:</i> from <b>hand-coding only</b> to <b>verified AI pairing</b><br/>Prompts · review · reject · small diffs"]
    end

    subgraph value ["Course & Real-Life Value"]
        CV["<b>Course Value</b><br/>Safe AI use before React scaffolding<br/>Industry-standard assistant workflow"]
        RL["<b>Real-Life Use</b><br/>Faster boilerplate · Debug help · Without shipping bugs"]
    end

    subgraph future ["Upcoming Modules"]
        direction TB
        U1["<b>Upcoming Module</b><br/>Module 5: React<br/><i>[Vite · Components]</i><br/>Component-based UI"]
        U2["<b>Upcoming Module</b><br/>Module 5: React<br/><i>[State · Tailwind]</i><br/>Interactive apps"]
        U3["<b>Upcoming Module</b><br/>Module 6: FastAPI<br/><i>[Python · APIs]</i><br/>Full-stack with AI review"]
    end

    P1 ==>|&nbsp;Foundation&nbsp;| CM
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

    class P1,CM previous
    class CS current
    class CV,RL value
    class U1,U2,U3 future

    linkStyle default stroke-width:3px
```
