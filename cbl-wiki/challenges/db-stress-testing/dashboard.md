---
type: dashboard
tags: [cbl, moc]
---

# Dashboard — Database Stress-Testing & Monitoring

**Mode:** Learn · **Phase:** Investigate
**Concepts:** 31 · **Questions:** 25 answered / 27 open · **Sources:** 13 · **Synthesis:** 0

🗺 [[knowledge-map.canvas|Canvas map]] · 🧭 [[learning-path|Learning path (Mermaid)]] · 🎞 [[presentation|Slide deck]] · 🎯 [[challenge]]

---

## Concepts by cluster

> The same 7 clusters as the [[knowledge-map.canvas|Canvas]]. Each line is a learnable unit.

### ① Foundations
[[database-vs-file]] · [[sql-basics]] · [[tables-rows-columns]] · [[primary-foreign-keys]]

### ② Indexing & Queries
[[database-index]] · [[b-tree-index]] · [[explain-vs-explain-analyze]] · [[slow-query-log]]

### ③ Measuring Performance
[[tps]] · [[latency-percentiles]] · [[latency-vs-throughput]] · [[tail-latency-amplification]] · [[slo-sla-targets]]

### ④ Bottlenecks & Stress Testing
[[database-bottleneck]] · [[stress-load-benchmark]] · [[stress-test-tools]] · [[pgbench-default-workload]]

### ⑤ Scaling Solutions (local)
[[cache-redis]] · [[cache-hit-ratio]] · [[cache-hit-ratio-vs-disk-io]] · [[message-queue]] · [[pgbouncer]] · [[connection-pool-size]]

### ⑥ Monitoring & Observability
[[prometheus]] · [[grafana]] · [[managed-db-monitoring-gaps]]

### ⑦ Scaling Out (Distributed)
[[scaling-up-vs-out]] · [[cassandra-distributed-architecture]] · [[scylladb-vs-cassandra]] · [[cql-query-language]] · [[discord-database-evolution]]

---

## Open questions (27) — the research frontier

These are still `status: open` — your next investigation targets.

- **Knowledge:** [[what-is-a-transaction-acid]] · [[postgresql-vs-other-databases]] · [[what-is-a-query-execution]] · [[what-is-a-connection-pool]] · [[too-many-users-at-once]] · [[lock-contention]] · [[what-is-a-deadlock]] · [[n-plus-one-query-problem]] · [[what-is-disk-io]] · [[cap-theorem]]
- **Skills:** [[setup-postgres-with-docker]] · [[seed-database-with-fake-data]] · [[run-basic-stress-test-pgbench]] · [[read-pgbench-output]] · [[use-explain-analyze]] · [[spot-a-seq-scan]] · [[add-index-and-verify]] · [[active-vs-max-connections]] · [[enable-pg-stat-statements]] · [[read-pg-stat-activity]]
- **Impact:** [[establish-a-baseline]] · [[numbers-that-prove-a-fix]] · [[draw-architecture-diagram]]
- **Correlations:** [[missing-index-vs-pool-exhaustion]] · [[latency-vs-throughput-tradeoff]] · [[fix-db-vs-cache-vs-queue]]
- **Limitations:** [[local-docker-vs-production]]

> ⚠️ Heavily **Skills-weighted**: 10 of 27 open questions are hands-on (set up Docker, run pgbench, add an index and verify). Your *knowledge* is well ahead of your *practice* — the Act phase is where these close.

---

## 🔎 Gap analysis

Findings from a link scan of the vault (2026-06-24):

1. **Synthesis is empty (0 pages).** You have 31 concepts but haven't consolidated them. The clusters above are *de facto* syntheses waiting to be written — run `/project:cbl:synthesize`. Strongest candidate: **"The bottleneck → three local fixes → scale out"** arc, which already recurs across ④⑤⑦.

2. **3 orphan sources — read but never cited:**
   - [[docker-getting-started]] — no concept links it. Will be cited the moment you answer [[setup-postgres-with-docker]].
   - [[use-the-index-luke]] — no concept links it, yet you have [[database-index]]/[[b-tree-index]]. Mine it for a worked indexing example.
   - [[hussein-nasser]] — no concept links it. Either extract a concept or drop it.

3. **One source is load-bearing.** [[designing-data-intensive-applications]] backs **9** concepts — almost all of ③ and ⑦. Healthy, but single-source: corroborate the latency/scaling claims with a second source before the Act phase.

4. **Weakly-linked concept:** [[managed-db-monitoring-gaps]] connects only to [[prometheus]]. It belongs near [[grafana]] and the monitoring skills too — add links.

5. **Practice gap (the big one).** 0 of the 10 Skills questions are answered. Everything is theory so far. Highest-leverage next move: stand up Postgres in Docker, run pgbench, and watch a bottleneck appear on a Grafana dashboard — that single exercise closes ~6 open questions at once.

---

## Suggested next steps

1. `/project:cbl:synthesize` → write the bottleneck→fix→scale-out conclusion
2. `/project:cbl:investigate` (Skills focus) → Docker + pgbench hands-on
3. Cite or cut the 3 orphan sources
4. When practice catches up to theory → `/project:cbl:act`

---
**Related:** [[index]] · [[log]] · [[challenge]]
