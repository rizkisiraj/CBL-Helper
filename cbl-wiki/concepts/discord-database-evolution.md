---
type: concept
status: answered
tags: [cbl]
---

# Discord's database evolution (case study)

A real-world example of how sustained growth forces database migrations — and what a
system at its limit looks like.

- **MongoDB** (from 2015): Discord's first store (NoSQL, horizontal scaling, reliable
  performance). Outgrew it as the platform got popular.
- **Cassandra** (2017): switched for better scalability, fault tolerance, flexibility,
  and high availability. Ran **12 nodes / billions of messages**.
- **By early 2022: 177 nodes / trillions of messages** — and Cassandra had become a
  **high-toil system**: on-call team frequently paged, **unpredictable latency**, and
  maintenance operations too expensive to run. Pain spanned scalability, performance,
  flexibility, consistency, and compatibility.
- **ScyllaDB**: the next move — Cassandra-compatible but faster and cheaper to run.
- **Go → Rust**: Discord also rewrote internal services in Rust for further
  performance gains beyond the database itself.

**Lesson for this challenge:** the symptoms above (frequent paging, unpredictable
latency, maintenance ops that no longer fit) are exactly the kind of real-world
**[[database-bottleneck]]** signals worth watching for in a stress-testing/monitoring
system — and a reminder that the fix is sometimes architectural (scale out / change
engine), not just a query tweak.

**Sources:** [[discord-database-transformation]]
**Related:** [[cassandra-distributed-architecture]] · [[scylladb-vs-cassandra]] · [[cql-query-language]] · [[database-bottleneck]]
