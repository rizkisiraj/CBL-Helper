# Knowing When a Database Will Break

### …and what to do before it does

A walk through the knowledge graph
**Big Idea:** Scalability

<small>Open with Obsidian → command palette → "Slides: Start presentation"</small>

---

## The Challenge

> How do I know when my current database design will **break**, and what do I do **before** it does?

Build a **stress-testing + monitoring system** that detects bottlenecks before they hit production — and use each bottleneck as a lens into the patterns that solve it at scale.

---

## The territory — 7 clusters

1. **Foundations** — what a DB even is
2. **Indexing & Queries** — why reads get slow
3. **Measuring Performance** — TPS, latency, percentiles
4. **Bottlenecks & Stress Testing** — find the breaking point
5. **Scaling Solutions (local)** — cache, queue, pool
6. **Monitoring** — Prometheus + Grafana
7. **Scaling Out** — when one box isn't enough

→ See `maps/knowledge-map.canvas` for the full graph

---

## 1 · Foundations

A database ≠ a file: concurrent access, validity rules, millions of records in ms.

- Data lives in **tables → rows → columns**
- **Primary keys** identify a row; **foreign keys** link tables → "relational"
- You talk to it with **SQL** (SELECT / INSERT / UPDATE / DELETE)

*Keys are the seed of the next idea: indexes.*

---

## 2 · Why reads get slow

Scanning a table to find a row is **O(n)** — double the rows, double the time.

- An **index** turns that into **O(log n)** — usually a **B-tree** (sorted pages, wide & shallow, a few page reads per lookup)
- The trade-off: indexes **speed reads but slow writes** — every insert updates every index
- Diagnose with the **slow query log** → then **EXPLAIN ANALYZE** to see the real plan

---

## 3 · You can't fix what you can't measure

Two different things — you can have one good and the other terrible:

- **Throughput / TPS** — operations finished per second
- **Latency / response time** — what *one* request feels

**Averages lie.** Use **percentiles**: p50, p95, p99.
The slow tail is right-skewed and hits your most valuable users.

---

## 3 · The tail bites harder than it looks

**Tail latency amplification:**

- A server runs only ~(CPU cores) in parallel → a few slow requests block everything behind them (**head-of-line blocking**)
- One user request that fans out to N backend calls waits for the **slowest** one
- So a *tiny* fraction of slow calls makes a *large* fraction of users slow

→ This is why p99 matters, and why your load generator must fire **independently** of replies (or it lies to you).

---

## 4 · Find the breaking point

- **Load** test → can it handle expected traffic?
- **Stress** test → ramp until it breaks
- **Benchmark** → standardized comparison

Start with **pgbench** (built into Postgres, zero setup).
⚠️ Its default TPC-B workload isn't realistic — graduate to custom `-f` scripts.

**The tell:** when TPS drops as you add users, you've found a **bottleneck**.

---

## 4 → 5 · A bottleneck has three local fixes

The bottleneck is the narrowest part of the pipe. Before re-architecting, reach for:

| Fix | When | Tool |
|---|---|---|
| **Index** | full table scans | B-tree |
| **Cache** | reads ≫ writes, repeated work | Redis |
| **Queue** | bursty writes | Kafka/SQS |
| **Pool** | too many connections | PgBouncer |

*Caveat: don't cache to mask a missing index. Fix the query first.*

---

## 5 · Caching & pooling, concretely

- **Cache hit ratio** target **99%+**; below 90% your data exceeds RAM
- Hit ratio and **disk I/O are inverse** — I/O spiking? check hit ratio first
- Each PG connection costs **5–10 MB** → **PgBouncer** reuses a small real pool
- More connections ≠ more speed: `pool ≈ cores × 2`, then **load test for the sweet spot**

---

## 6 · See it happening

- **Prometheus** scrapes metrics every ~15s (`postgres_exporter` at :9187)
- **Grafana** turns them into dashboards: TPS, connections, hit ratio, p95/p99, locks
- **Managed DBs (RDS) hide the OS** — no raw disk I/O, no htop; you trade visibility for ops simplicity

---

## 7 · When one box isn't enough

- **Scale up** (bigger machine): simple, but a price & ceiling exists
- **Scale out** (more machines, shared-nothing): necessary at extreme load
- DDIA's rule: **keep the DB on one node until cost or HA forces you out.** No magic scaling sauce.

---

## 7 · Proof from the real world: Discord

- 2015 **MongoDB** → 2017 **Cassandra** (12 nodes, billions of msgs)
- 2022: **177 nodes, trillions of msgs** → frequent paging, **unpredictable latency**, maintenance too costly
- → **ScyllaDB** (C++, no GC pauses; CQL-compatible drop-in)

**The symptoms are the lesson:** paging, tail latency, ops that no longer fit = the bottleneck signals worth monitoring for.

---

## When are you *done*?

You don't chase "fast" — you set **SLOs**:

> p50 < 200 ms · p99 < 1 s · up ≥ 99.9%

- Targets are **percentiles**, because the tail is what users feel
- **Diminishing returns** at the extreme tail — Amazon won't optimize p99.99
- "Good enough" = hit the SLO, then **stop**

---

## The through-line

**Foundations** → **Indexing** → you **Measure** → **Stress-test to find the bottleneck** → fix it **locally (cache/queue/pool)** → **Monitor** it → and only when that runs out, **Scale out**. Judge every fix against an **SLO**.

→ Explore the live graph: `maps/knowledge-map.canvas`
→ Next: **synthesize** these 31 concepts into conclusions
