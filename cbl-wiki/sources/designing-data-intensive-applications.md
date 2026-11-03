---
type: source
raw_file: raw/Martin-Kleppmann---Designing-Data-Intensive-Applications_-O’Reilly-Media-(2017).pdf
url: https://dataintensive.net/
tags: [cbl]
---

# Designing Data-Intensive Applications

Martin Kleppmann (O'Reilly, 2017) — the standard reference for databases & distributed
systems. ~490 pages across three parts: Foundations of Data Systems (reliability,
scalability, maintainability; data models; storage engines; encoding), Distributed Data
(replication, partitioning, transactions, consistency), and Derived Data (batch & stream
processing). **Compiled selectively** against this challenge's focus — the chapters on
performance and storage engines, not the whole book.

## Compiled sections & key claims

**Ch. 1 — Describing performance (pp. 11–16):**
- Distinguishes **latency** (a request waiting, *latent*, awaiting service) from
  **response time** (what the client actually sees = service time + network + queueing
  delays). Often conflated; not the same.
- **Throughput** (records/sec) is the metric for batch systems; **response time** matters
  for online systems.
- Response time is a *distribution*, not a number. **The mean misleads** — use
  **percentiles**: median/p50, p95, p99, p999. "p95 = 1.5s" means 95% of requests are
  faster than 1.5s.
- **High percentiles (tail latency) matter** because the slowest requests often hit the
  most valuable users. Amazon: 100ms slower → 1% fewer sales; optimizes to p99.9 but
  not p99.99 (diminishing returns, dominated by uncontrollable random events).
- Percentiles feed **SLOs/SLAs** (e.g. p50 < 200ms, p99 < 1s, up 99.9% of the time).
- **Queueing delays** dominate high percentiles; a few slow requests cause
  **head-of-line blocking**. One slow backend among parallel calls slows the whole
  end-user request (tail amplification). **Measure on the client side.**
- Load-testing clients must **keep sending requests independently** of response time —
  waiting for the previous reply artificially shortens queues and skews results.

**Ch. 1 — Coping with load (pp. 15–16):** scaling **up** (vertical, bigger machine) vs
**out** (horizontal, distribute across machines = *shared-nothing*). Elastic vs manual
scaling. No one-size-fits-all "magic scaling sauce"; common wisdom: keep a database on a
single node until cost or HA forces distribution.

**Ch. 3 — Storage & retrieval (pp. 67–104):**
- An **index** is an additional structure *derived from* the primary data. Fundamental
  trade-off: **well-chosen indexes speed up reads, but every index slows down writes** —
  so DBs don't index everything by default; you pick indexes from query patterns.
- A full scan is **O(n)**; an index turns lookups into roughly **O(log n)**.
- **B-trees** are the standard relational index: fixed-size pages (~4 kB) forming a
  balanced tree of height O(log n); update = overwrite a page in place. (Contrast:
  log-structured/LSM-trees only append.)

**Taught:** [[latency-percentiles]] · [[latency-vs-throughput]] ·
[[tail-latency-amplification]] · [[slo-sla-targets]] · [[scaling-up-vs-out]] ·
[[database-index]] · [[b-tree-index]] · [[connection-pool-size]] · [[database-bottleneck]]
