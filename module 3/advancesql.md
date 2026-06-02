```mermaid id="advanced_sql_joins"
%%{init: {
  "flowchart": {
    "nodeSpacing": 40,
    "rankSpacing": 70,
    "diagramPadding": 20
  }
}}%%

flowchart TB

subgraph Foundation["🚀 Foundation Built So Far"]
    P1["Previous Module<br/><b>Database Fundamentals I</b><br/>(Tables, Keys)<br/>Database Structure Design"]

    P2["Previous Module<br/><b>Database Fundamentals II</b><br/>(UPDATE, DELETE)<br/>Managing Existing Records"]

    P3["Previous Module<br/><b>Database Fundamentals II</b><br/>(WHERE, Relationships)<br/>Filtering & Data Connections"]
end

subgraph Present["🎯 Current Learning Focus"]
    CM["Current Module Until Previous Session<br/><b>Database Foundations</b><br/>Tables • CRUD • Relationships"]

    CS{{"Current Session<br/><b>Advanced SQL: JOINs</b><br/>Connecting Related Tables<br/>INNER • LEFT • RIGHT • FULL JOIN<br/>Single View from Multiple Tables"}}
end

subgraph Value["💡 Why This Matters"]
    CV["Course Value<br/><b>Relational Database Mastery</b><br/>Query Data Across Systems"]

    RV["Real-Life Value<br/><b>Business Reporting & Analytics</b><br/>Customers + Orders<br/>Students + Courses<br/>Products + Sales"]
end

subgraph Future["🔮 What's Coming Next"]
    U1["Upcoming Module<br/><b>SQL Aggregation</b><br/>(COUNT, SUM, AVG)<br/>Data Analysis & Metrics"]

    U2["Upcoming Module<br/><b>SQL Practice</b><br/>(Queries, Debugging)<br/>Real-World Problem Solving"]

    U3["Upcoming Module<br/><b>SQLAlchemy ORM</b><br/>(ORM, Relationships)<br/>FastAPI ↔ Database Integration"]
end

P1 ==>|&nbsp;Data Model&nbsp;| P2
P2 ==>|&nbsp;Manage Data&nbsp;| P3
P3 ==>|&nbsp;Relationships&nbsp;| CM

CM ==>|&nbsp;Connect Tables&nbsp;| CS

CS ==>|&nbsp;Course Path&nbsp;| CV
CS ==>|&nbsp;Industry Use&nbsp;| RV

CS ==>|&nbsp;Next Module&nbsp;| U1
U1 ==>|&nbsp;Practice Skills&nbsp;| U2
U2 ==>|&nbsp;Backend Usage&nbsp;| U3

classDef previous fill:#E8F4FD,stroke:#5B9BD5,stroke-width:2px,color:#111;
classDef current fill:#FFF4CC,stroke:#D6A800,stroke-width:4px,color:#111;
classDef value fill:#EAF7EA,stroke:#67B567,stroke-width:2px,color:#111;
classDef future fill:#F4ECFF,stroke:#8A6FD1,stroke-width:2px,color:#111;

class P1,P2,P3,CM previous;
class CS current;
class CV,RV value;
class U1,U2,U3 future;

linkStyle default stroke-width:3px
```
