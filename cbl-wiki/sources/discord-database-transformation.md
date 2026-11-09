---
type: source
raw_file: raw/discord-database-article.md
url: https://medium.com/@siddharthgov01/case-study-discords-database-transformation-a8d5b226a7ab
tags: [database, system-design, distributed-systems]
---

# Case Study: Discord's Database Transformation (Siddharth, Medium)

A short case study (Apr 2023) walking through why Discord changed databases as it
grew, and the engineering tradeoffs behind each move.

**Main claims & facts:**
- Discord (launched 2015) started on **MongoDB** (NoSQL, horizontal scaling), then
  migrated to **Cassandra** in 2017, and later to **ScyllaDB** — each move driven by
  scaling and performance pressure.
- In 2017 Discord ran **12 Cassandra nodes** storing billions of messages; by early
  2022 it had **177 nodes** storing trillions of messages and had become a
  "high-toil" system (on-call frequently paged, unpredictable latency, maintenance
  ops too expensive to run).
- Cassandra is a distributed NoSQL DBMS (originally Facebook, now Apache): equal
  nodes (~2–4 TB each), grouped into clusters/"rings", partitioned via a distributed
  hash ring + partition key, replicated across nodes/datacenters, coordinated by any
  node, using a gossip protocol, memtable, and columnar storage.
- **CQL** (Cassandra Query Language) is SQL-like but **does not support JOINs**.
- **ScyllaDB** is Cassandra-/DynamoDB-API compatible, supports CQL, and is faster:
  written in **C++** (no garbage collector) vs Cassandra's **Java**; lower TCO,
  higher ROI; managed via Scylla Manager.
- Discord also switched internal services from **Go to Rust** for further performance.

Cites Discord's own engineering blogs ("How Discord Stores Trillions of Messages").

**Taught:** [[discord-database-evolution]] · [[cassandra-distributed-architecture]] · [[scylladb-vs-cassandra]] · [[cql-query-language]]
