---
type: concept
status: answered
tags: [cbl]
---

# Stress-testing tools

pgbench (built into PostgreSQL, zero setup — start here), sysbench, HammerDB, k6, JMeter. Begin: pgbench -i mydb (initialize), then pgbench -c 10 -T 60 mydb (10 clients, 60s). Graduate to custom -f scripts for realistic workloads.

**Answers:** [[stress-testing-tools-for-beginners]]
**Sources:** [[postgresql-docs]] · [[pgbench-tutorial]]
**Related:** [[pgbench-default-workload]] · [[stress-load-benchmark]]
