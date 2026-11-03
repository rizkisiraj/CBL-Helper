---
type: concept
status: answered
tags: [cbl]
---

# SLOs / SLAs — when is performance "good enough"

You don't chase "fast" — you set explicit, percentile-based targets and stop when you hit them.

- An **SLO** (service level objective) is the internal target; an **SLA** is the contractual version (with refunds if missed). Both are stated as **percentiles + availability**, e.g. *p50 < 200 ms, p99 < 1 s, up ≥ 99.9% of the time*.
- Targets use percentiles, not averages, because the [[latency-percentiles|tail]] is what users feel. Amazon specs internal services at p99.9 — 1 in 1,000 requests — because the slowest requests often hit the most valuable customers (100 ms slower ≈ 1% fewer sales).
- **Diminishing returns at the extreme tail.** Amazon deliberately does *not* optimize p99.99: those are dominated by random events outside your control, so the cost outweighs the benefit. "Good enough" = meet the SLO, then stop.

Practical read for this challenge: pick concrete numbers (e.g. p95 query latency + a minimum [[tps]]) up front, then judge every fix against them rather than optimizing forever.

**Answers:** [[performance-good-enough-targets]]
**Sources:** [[designing-data-intensive-applications]]
**Related:** [[latency-percentiles]] · [[tail-latency-amplification]] · [[latency-vs-throughput]]
