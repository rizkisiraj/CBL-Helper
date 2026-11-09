---
type: concept
status: answered
tags: [database, system-design, bridge]
---

# p95 / p99 latency

P95/P99 means 95%/99% of requests finished faster than this time. Averages lie — one 10s request hides inside a low mean. Latency distributions are right-skewed with a long slow tail, which compounds across parallel calls.

DDIA frames this precisely: response time is a *distribution*, not a number. The **mean** doesn't tell you how many users actually experienced a delay, so use percentiles — median (p50), p95, p99, p999. High percentiles are worth tracking because the slowest requests often hit the most valuable users; they also feed [[slo-sla-targets]]. Because high percentiles are dominated by queueing, they amplify across calls — see [[tail-latency-amplification]].

## Under the hood (DSA)
- A latency histogram sorts requests into fixed time buckets by index to estimate percentiles cheaply — the same bucketing idea as [[bucket-sort]].
- Streaming the running median/percentile of incoming latencies without re-sorting on every request is the classic use case for a [[heap-priority-queue]].

**Answers:** [[p95-p99-latency]]
**Sources:** [[designing-data-intensive-applications]]
**Related:** [[tps]] · [[latency-vs-throughput]] · [[tail-latency-amplification]] · [[slo-sla-targets]]
