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
        M3["<b>Previous Module</b><br/><i>Module 3</i><br/>[FastAPI · SQL · ORM]<br/>REST APIs, backend services"]:::prev
        LLM["<b>Previous Session</b><br/><i>Intro to LLMs</i><br/>[Transformers · Tokenization]<br/>How models process text"]:::prev
        PE["<b>Current Module — Until Previous Session</b><br/>Module 4 · Applied AI<br/>Prompt Engineering<br/>Zero-shot · Few-shot · CoT · Role prompting"]:::currmod
    end

    M3 ==>|&nbsp;API foundation&nbsp;| LLM
    LLM ==>|&nbsp;Model intuition&nbsp;| PE

    subgraph NOW ["⚡ &nbsp; NOW"]
        CURR["★ &nbsp;<b>CURRENT SESSION</b><br/>AI API Integration<br/>Auth · Chat roles · Streaming responses<br/>FastAPI + LLM service · Reusable API functions"]:::current
    end

    PE ==>|&nbsp;Prompts meet real APIs&nbsp;| CURR

    subgraph VALUE ["💡 &nbsp; WHY IT MATTERS"]
        direction LR
        CV["<b>Course Value</b><br/>Bridges backend skills with AI capabilities<br/>Foundation for every AI feature ahead"]:::value
        RV["<b>Real-Life Value</b><br/>Ship AI-powered products<br/>Chat apps, assistants, automation"]:::value
    end

    CURR ==>|&nbsp;Course path&nbsp;| CV
    CURR ==>|&nbsp;Real-life use&nbsp;| RV

    subgraph UPCOMING ["🚀 &nbsp; UPCOMING MODULES"]
        direction LR
        U1["<b>Upcoming — Module 4</b><br/><i>Advanced AI Integration</i><br/>[Vision · Embeddings · Schemas]<br/>Structured outputs, multimodal inputs"]:::future
        U2["<b>Upcoming — Module 4</b><br/><i>Claude Code</i><br/>[CLI · Agentic dev]<br/>AI-assisted project building"]:::future
        U3["<b>Upcoming — Module 4</b><br/><i>Capstone Project</i><br/>[Full-stack · Agents]<br/>Ship an end-to-end AI app"]:::capstone
    end

    CV ==>|&nbsp;Next session&nbsp;| U1
    RV ==>|&nbsp;Next session&nbsp;| U1
    U1 ==>|&nbsp;Level up&nbsp;| U2
    U2 ==>|&nbsp;Build with it&nbsp;| U3
```