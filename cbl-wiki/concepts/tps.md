---
type: concept
status: answered
tags: [database, system-design, bridge]
---

# TPS (transactions per second)

How many complete DB operations the system processes each second. Measures throughput, not latency. The first number to watch when stress testing — if TPS drops as you add users, you've found a bottleneck.

## Under the hood (DSA)
- TPS is computed over a moving time window, the same [[sliding-window]] idea applied to a metrics stream instead of a string.
- The counter behind it is cumulative; the rate is the difference of two running totals, like the running-prefix trick in [[prefix-suffix-products]].

**Answers:** [[what-is-tps]]
**Sources:** [[pgbench-tutorial]]
**Related:** [[latency-percentiles]] · [[database-bottleneck]]
