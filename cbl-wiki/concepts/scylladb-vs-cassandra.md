---
type: concept
status: answered
tags: [cbl]
---

# ScyllaDB vs Cassandra

ScyllaDB is a fast, scalable NoSQL database that is a near drop-in for Cassandra:
it's **compatible with the Cassandra and Amazon DynamoDB APIs** and **supports CQL**,
and it works the same way under the hood (data replicated across nodes grouped in a
cluster, coordinator node handling reads/writes/replication).

Why teams (including Discord) move from Cassandra to ScyllaDB:

1. **Written in C++ (no garbage collector)** vs Cassandra's **Java**, whose GC pauses
   make it slower — the headline performance win.
2. High availability, flexibility, scalability, and fault tolerance.
3. **Low TCO** (total cost of ownership) — fewer hardware resources, lower
   operational cost.
4. **High ROI**.
5. Compatibility (Cassandra + DynamoDB APIs, CQL) eases migration.
6. Easier ops via **Scylla Manager**: automated cluster management, rolling upgrades,
   backups, and monitoring.

Used by Discord, Disney+ Hotstar, Starbucks, Samsung, IBM, and others; markets itself
on millions of ops/second and petabyte scaling.

**Sources:** [[discord-database-transformation]]
**Related:** [[cassandra-distributed-architecture]] · [[cql-query-language]] · [[discord-database-evolution]]
