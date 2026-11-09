---
type: map
tags: [database, system-design, distributed-systems]
---

# Learning Path & System Map

Two views of the same knowledge. Open in Obsidian (reading mode) to render the diagrams.
For the draggable, note-embedded version see [[knowledge-map.canvas|the Canvas map]].

## 1 · Concept dependency path

What to understand before what — follow the arrows.

```mermaid
flowchart TD
    F["①Foundations<br/>db vs file · SQL · tables · keys"]
    IDX["②Index<br/>O(n)→O(log n) · B-tree"]
    DIAG["EXPLAIN ANALYZE<br/>+ slow query log"]
    MEAS["③Measure<br/>TPS · latency · p95/p99"]
    TAIL["Tail amplification<br/>head-of-line blocking"]
    STRESS["④Stress test<br/>pgbench → find bottleneck"]
    BN{{"Bottleneck<br/>found"}}
    CACHE["⑤Cache · Queue · Pool<br/>local fixes"]
    MON["⑥Monitor<br/>Prometheus → Grafana"]
    OUT["⑦Scale out<br/>Cassandra → ScyllaDB"]
    SLO["SLO<br/>p99 target → stop"]

    F --> IDX --> DIAG
    F --> MEAS
    MEAS --> TAIL
    DIAG --> STRESS
    MEAS --> STRESS
    STRESS --> BN
    BN -->|reads ≫ writes| CACHE
    BN -->|missing index| IDX
    BN -->|one box maxed| OUT
    BN --> MON
    TAIL --> SLO
    CACHE --> SLO
    OUT --> SLO
    MON --> SLO

    classDef found fill:#7b6cf6,color:#fff,stroke:#fff
    classDef know fill:#3aa757,color:#fff,stroke:#fff
    classDef meas fill:#e08a1e,color:#fff,stroke:#fff
    classDef bn fill:#d23f3f,color:#fff,stroke:#fff
    classDef sol fill:#2aa6b8,color:#fff,stroke:#fff
    classDef mon fill:#c9a227,color:#fff,stroke:#fff
    class F found
    class IDX,DIAG know
    class MEAS,TAIL,SLO meas
    class STRESS,BN bn
    class CACHE,OUT sol
    class MON mon
```

## 2 · The system you're stress-testing

Where every concept physically sits in the request path.

```mermaid
flowchart LR
    U["Users<br/>(spikes)"]
    LG["Load generator<br/>pgbench / k6"]
    Q["Message queue<br/>absorbs write spikes"]
    POOL["PgBouncer<br/>connection pool"]
    C["Cache (Redis)<br/>&lt;1ms reads"]
    DB[("PostgreSQL<br/>+ indexes")]
    DISK[("Disk<br/>I/O")]
    EXP["postgres_exporter"]
    PROM["Prometheus"]
    GRAF["Grafana<br/>TPS · p99 · hit ratio"]

    U --> POOL
    LG -.simulates load.-> POOL
    U -->|bursty writes| Q --> POOL
    POOL --> C
    C -->|miss| DB
    POOL --> DB
    DB -->|cache miss| DISK
    DB --> EXP --> PROM --> GRAF
```

## 3 · The decision: which fix?

```mermaid
flowchart TD
    S["TPS dropping /<br/>latency climbing"] --> Q1{"Disk I/O<br/>high?"}
    Q1 -->|yes| Q2{"Cache hit<br/>ratio low?"}
    Q1 -->|no| Q3{"Connections<br/>maxed?"}
    Q2 -->|yes| A1["Add cache /<br/>raise shared_buffers"]
    Q2 -->|no| A2["Missing index →<br/>EXPLAIN ANALYZE"]
    Q3 -->|yes| A3["PgBouncer /<br/>tune pool size"]
    Q3 -->|no| A4["Query/lock issue →<br/>slow query log"]
    A1 --> Z{"Still over<br/>SLO at scale?"}
    A2 --> Z
    A3 --> Z
    A4 --> Z
    Z -->|yes| OUT["Scale out<br/>(distributed)"]
    Z -->|no| DONE["Meet SLO → stop"]
```

---
**Related:** [[index]] · [[challenge]] · [[knowledge-map.canvas]]
