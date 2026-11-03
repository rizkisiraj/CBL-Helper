---
type: concept
status: answered
tags: [cbl]
---

# PgBouncer

Lightweight connection pooler. Each PostgreSQL connection uses 5-10MB RAM, so 200 direct connections cost 1-2GB. PgBouncer reuses a small pool of real connections. Need it above ~50 concurrent connections or in serverless environments. Transaction pooling is best for web apps.

**Answers:** [[what-is-pgbouncer]]
**Sources:** [[pgbouncer-docs]]
**Related:** [[connection-pool-size]]
