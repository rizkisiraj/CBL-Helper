---
type: concept
status: answered
tags: [database, system-design, distributed-systems]
---

# Scaling up vs scaling out (DDIA framing)

The canonical vocabulary behind [[horizontal-vs-vertical-scaling]]:

- **Scaling up (vertical):** move to a more powerful machine. Simpler — a single-node system has no distribution complexity — but high-end machines get very expensive, and there's a ceiling.
- **Scaling out (horizontal):** spread load across many smaller machines, a **shared-nothing** architecture. Necessary for very intensive workloads; this is what [[cassandra-distributed-architecture|Cassandra-style clusters]] and [[discord-database-evolution|Discord's migration]] do.
- Real architectures **mix both** — a few fairly powerful machines often beat a swarm of tiny VMs.
- **Elastic** systems add resources automatically on load; **manually** scaled systems are simpler with fewer surprises.
- DDIA's caution: there's **no one-size-fits-all "magic scaling sauce."** Distributing *stateless* services is easy; taking a *stateful* database from one node to many adds real complexity. Common wisdom — **keep the database on a single node (scale up) until cost or high-availability needs force you to distribute.** Premature scaling for hypothetical future load is often wasted effort.

**Sources:** [[designing-data-intensive-applications]]
**Related:** [[horizontal-vs-vertical-scaling]] · [[cassandra-distributed-architecture]] · [[discord-database-evolution]] · [[database-bottleneck]]
