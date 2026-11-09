---
type: concept
status: answered
tags: [database, system-design]
---

# TPS (transactions per second)

How many complete DB operations the system processes each second. Measures throughput, not latency. The first number to watch when stress testing — if TPS drops as you add users, you've found a bottleneck.

**Answers:** [[what-is-tps]]
**Sources:** [[pgbench-tutorial]]
**Related:** [[latency-percentiles]] · [[database-bottleneck]]
