---
type: concept
status: answered
tags: [cbl]
---

# Latency vs throughput (and response time)

They measure different things, so you can have one good and the other bad.

- **Throughput** = how many operations finish per second (DDIA: records/sec; in DB land, [[tps]]). The metric that matters for batch work.
- **Response time** = what the client sees for *one* request, end to end = service time + network delay + queueing delay. The metric that matters for online/interactive work.
- **Latency** (DDIA's strict sense) = just the time a request sits *latent*, waiting to be handled — a component of response time, not a synonym.

**Yes, you can have high throughput but bad latency.** A system can push huge TPS while individual requests sit in long queues — throughput counts completions, queueing inflates each request's response time (and especially the tail, see [[latency-percentiles]] / [[tail-latency-amplification]]). Batch systems optimize throughput; user-facing systems must also watch the latency distribution.

**Answers:** [[latency-vs-throughput-tradeoff]]
**Sources:** [[designing-data-intensive-applications]]
**Related:** [[tps]] · [[latency-percentiles]] · [[tail-latency-amplification]] · [[database-bottleneck]]
