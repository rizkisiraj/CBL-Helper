---
type: concept
status: answered
tags: [database, system-design, distributed-systems]
---

# Cassandra's distributed architecture (horizontal scale-out)

Cassandra is a distributed NoSQL DBMS (originally built at Facebook, now Apache
open source) designed to hold huge data across many servers with high availability
and fault tolerance. Instead of one ever-bigger server (vertical scaling), it
**scales horizontally** by adding more machines:

- Each instance is a **Node** (~2–4 TB of data). Nodes are grouped into a
  **Cluster** (a.k.a. "Ring").
- **No master/coordinator node** — every node plays an equal role.
- A **distributed hash ring** partitions data across nodes by **partition key**;
  each partition is owned by a specific node.
- Writes go to the owning node, which acts as **coordinator** and **replicates** the
  data to other nodes — often across multiple datacenters for fault tolerance, so no
  single point of failure loses data.
- Any node can coordinate a client read/write, routing it to the right replica and
  managing query execution and consistency.
- A **memtable** holds recently written data before it is persisted to disk.
- Nodes share cluster status and replica locations via a **gossip protocol**.
- Storage is **columnar**; it also supports **sharding**.

This is the concrete answer to "where do you go when a bigger server isn't enough":
add nodes to the ring rather than scaling one box up.

**Answers:** [[horizontal-vs-vertical-scaling]]
**Sources:** [[discord-database-transformation]]
**Related:** [[scylladb-vs-cassandra]] · [[cql-query-language]] · [[discord-database-evolution]] · [[database-bottleneck]]
