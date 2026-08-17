```mermaid
flowchart TB
    subgraph foundation ["Foundation"]
        direction TB
        P1["<b>Previous Module</b><br/>Module 1: Programming Foundations<br/><i>[Python · DSA]</i><br/>Data structures · search · sort"]
        CM["<b>Current Module Until Previous Session</b><br/>Module 1: Developer Setup<br/><i>[Python · VS Code · Modules]</i><br/>Local install · imports · multi-file scripts"]
    end

    subgraph current ["Current Session"]
        CS["<b>Current Session</b><br/>Project Structure, pip & venv<br/><i>Mental shift:</i> from <b>loose scripts</b> to <b>structured projects</b><br/>Folders · venv · pip · text-file utility"]
    end

    subgraph value ["Course & Real-Life Value"]
        CV["<b>Course Value</b><br/>Professional layout before JSON and Git<br/>Dependency isolation for all later modules"]
        RL["<b>Real-Life Use</b><br/>Team repos · requirements.txt · reproducible dev setups · CLI utilities"]
    end

    subgraph future ["Upcoming Modules"]
        direction TB
        U1["<b>Upcoming Module</b><br/>Module 1: Developer Setup<br/><i>[JSON · Files · Exceptions]</i><br/>Persistent data · error handling"]
        U2["<b>Upcoming Module</b><br/>Module 2: Web Fundamentals<br/><i>[HTML · CSS · JavaScript]</i><br/>Frontend pages and styling"]
        U3["<b>Upcoming Module</b><br/>Module 3: FastAPI Backend<br/><i>[REST · Pydantic]</i><br/>API development"]
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
