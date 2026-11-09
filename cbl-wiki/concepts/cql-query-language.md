---
type: concept
status: answered
tags: [database, system-design, distributed-systems]
---

# CQL (Cassandra Query Language)

CQL is the query language used to interact with Cassandra (and ScyllaDB, which also
supports it). It is **similar to SQL** — same familiar shape for the common
operations:

- **Insert / update** with a `WHERE` clause:
  `INSERT INTO users (...) VALUES (...);` / `UPDATE users SET ... WHERE user_id = 1;`
- **Select / filter**: `SELECT * FROM users WHERE age > 18 AND city = 'Mumbai';`
- **Batch insertions**: `BEGIN BATCH ... APPLY BATCH;`

The critical difference from SQL: **CQL does not support `JOIN` statements.** This is
a defining tradeoff of the distributed NoSQL model — you denormalize and design tables
around your queries instead of joining across normalized tables.

**Sources:** [[discord-database-transformation]]
**Related:** [[sql-basics]] · [[cassandra-distributed-architecture]] · [[scylladb-vs-cassandra]]
