```mermaid
flowchart TB
    subgraph foundation ["Foundation"]
        P1["<b>Previous Module</b><br/>Module 2: Web Fundamentals<br/><i>[JS · DOM]</i><br/>Manual tree updates"]
        P2["<b>Previous Module</b><br/>Module 2: Web Fundamentals<br/><i>[Fetch]</i><br/>GET JSON in the browser"]
        CM["<b>Current Module Until Previous Session</b><br/>Module 2: Web Fundamentals<br/><i>[Libraries vs frameworks]</i><br/>Why React-like UI tools exist"]
    end
    subgraph current ["Current Session"]
        CS["<b>Current Session</b><br/>React Fundamentals: Theory and Hooks<br/><i>Mental shift:</i> <b>imperative DOM</b> → <b>components and state</b><br/>Vite · JSX · props · useState · useEffect"]
    end
    subgraph value ["Course & Real-Life Value"]
        CV["<b>Course Value</b><br/>Minimum React for the CRUD lab and internships"]
        RL["<b>Real-Life Use</b><br/>Dashboards · design-system props · title sync"]
    end
    subgraph future ["Upcoming Modules"]
        U1["<b>Upcoming Module</b><br/>Module 2: Web Fundamentals<br/><i>[React CRUD]</i><br/>Todo plus localStorage plus GET"]
        U2["<b>Upcoming Module</b><br/>Module 3: FastAPI<br/><i>[REST backend]</i><br/>Replace JSONPlaceholder later"]
        U3["<b>Upcoming Module</b><br/>Module 4: LLM APIs<br/><i>[AI features]</i><br/>Effects that call model APIs"]
    end
    P1 ==>|&nbsp;Foundation&nbsp;| P2
    P2 ==>|&nbsp;Components&nbsp;| CM
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
