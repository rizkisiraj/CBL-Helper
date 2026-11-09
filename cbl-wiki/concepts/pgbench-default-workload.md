---
type: concept
status: answered
tags: [database, system-design]
---

# pgbench default workload

The default TPC-B-like workload uses uniform random access on small tables and updates one global row — good for baseline comparisons and lock-contention testing, but not representative of real apps (complex joins, uneven distribution, bursty traffic). Use custom -f scripts for realistic tests.

**Answers:** [[pgbench-default-workload-realistic]]
**Sources:** [[postgresql-docs]] · [[pgbench-tutorial]]
**Related:** [[stress-test-tools]]
