---
type: concept
status: answered
tags: [cbl]
---

# Tail latency amplification & head-of-line blocking

Why the slow tail (p99/p999) matters more than its rarity suggests.

- **Queueing dominates high percentiles.** A server processes only a few things in parallel (≈ CPU cores), so a handful of slow requests hold up everything behind them — **head-of-line blocking**. The waiting requests look slow to the client even though their own service time was fast.
- **Amplification across calls.** If one end-user request fans out to several backend calls (even in parallel), it must wait for the *slowest* one. So a small fraction of slow backend calls makes a much larger fraction of end-user requests slow.
- **Measure on the client side** — server-side timing misses the network and queueing delays the user actually feels.
- **Don't fool your own load test:** the load generator must keep firing requests *independently* of when responses come back. If it waits for each reply before sending the next, queues stay artificially short and the test understates real latency — directly relevant to running [[stress-load-benchmark]] / pgbench.

**Sources:** [[designing-data-intensive-applications]]
**Related:** [[latency-percentiles]] · [[latency-vs-throughput]] · [[slo-sla-targets]] · [[stress-load-benchmark]]
