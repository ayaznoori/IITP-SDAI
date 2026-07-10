```mermaid
flowchart TB
    classDef prev fill:#E8E7F0,stroke:#8B87CC,color:#3C3489,stroke-width:1.5px
    classDef currmod fill:#E1F5EE,stroke:#1D9E75,color:#085041,stroke-width:1.5px
    classDef current fill:#534AB7,stroke:#3C3489,color:#ffffff,stroke-width:3px
    classDef value fill:#FAEEDA,stroke:#BA7517,color:#633806,stroke-width:1.5px
    classDef future fill:#E6F1FB,stroke:#378ADD,color:#0C447C,stroke-width:1.5px
    classDef capstone fill:#FAECE7,stroke:#993C1D,color:#4A1B0C,stroke-width:1.5px

    linkStyle default stroke-width:2.5px

    subgraph FOUNDATIONS ["🏗 &nbsp; FOUNDATIONS"]
        direction LR
        M3["<b>Previous Module</b><br/><i>Module 3</i><br/>[FastAPI · SQL · ORM]<br/>Backend APIs, data models"]:::prev
        LLM["<b>Previous Session</b><br/><i>Intro to LLMs + Prompt Engineering</i><br/>[Transformers · CoT · Role prompting]<br/>Model intuition, shaping responses"]:::prev
        AIAPI["<b>Current Module — Until Previous Session</b><br/>Module 4 · Applied AI<br/>AI API Integration<br/>Auth · Chat roles · Streaming · FastAPI service"]:::currmod
    end

    M3 ==>|&nbsp;Backend foundation&nbsp;| LLM
    LLM ==>|&nbsp;Prompt skills&nbsp;| AIAPI

    subgraph NOW ["⚡ &nbsp; NOW"]
        CURR["★ &nbsp;<b>CURRENT SESSION</b><br/>Advanced AI Integration<br/>Structured outputs · Vision APIs · Embeddings<br/>Vector generation · Multimodal inputs and outputs"]:::current
    end

    AIAPI ==>|&nbsp;Go beyond text&nbsp;| CURR

    subgraph VALUE ["💡 &nbsp; WHY IT MATTERS"]
        direction LR
        CV["<b>Course Value</b><br/>Unlocks richer AI features in your capstone<br/>Images, search, and typed data from one API"]:::value
        RV["<b>Real-Life Value</b><br/>Semantic search, document AI, visual apps<br/>Skills used across modern AI products"]:::value
    end

    CURR ==>|&nbsp;Course path&nbsp;| CV
    CURR ==>|&nbsp;Real-life use&nbsp;| RV

    subgraph UPCOMING ["🚀 &nbsp; UPCOMING MODULES"]
        direction LR
        U1["<b>Upcoming — Module 4</b><br/><i>Claude Code for AI Dev</i><br/>[CLI · Agentic workflow]<br/>AI-assisted project building"]:::future
        U2["<b>Upcoming — Module 4</b><br/><i>Capstone Planning</i><br/>[Architecture · Schema · Auth]<br/>Design before you build"]:::future
        U3["<b>Upcoming — Module 4</b><br/><i>Capstone Project</i><br/>[Full-stack · Agents · Deploy]<br/>Ship an end-to-end AI app"]:::capstone
    end

    CV ==>|&nbsp;Next session&nbsp;| U1
    RV ==>|&nbsp;Next session&nbsp;| U1
    U1 ==>|&nbsp;Plan it&nbsp;| U2
    U2 ==>|&nbsp;Build it&nbsp;| U3
```