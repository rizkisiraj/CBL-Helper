---
type: hub
tags: [system-design]
---

# System Design

All `system-design`-tagged concepts — the patterns that apply once a single database
or single box stops being enough.

## Measuring performance
[[tps]] · [[latency-percentiles]] · [[latency-vs-throughput]] · [[slo-sla-targets]]

## Bottlenecks & stress testing
[[database-bottleneck]] · [[stress-load-benchmark]] · [[stress-test-tools]] · [[pgbench-default-workload]]

## Local scaling patterns
[[cache-redis]] · [[cache-hit-ratio]] · [[cache-hit-ratio-vs-disk-io]] · [[message-queue]] · [[pgbouncer]] · [[connection-pool-size]] · [[lru-cache]] · [[rate-limiting]]

## Monitoring & observability
[[prometheus]] · [[grafana]] · [[managed-db-monitoring-gaps]]

## Scaling out (distributed)
[[scaling-up-vs-out]] · [[cassandra-distributed-architecture]] · [[cql-query-language]] · [[discord-database-evolution]] · [[scylladb-vs-cassandra]] · [[tail-latency-amplification]] · [[lsm-tree]] · [[consistent-hashing]] — see [[distributed-systems]] for the full set.

## Where this is powered by DSA

See [[dsa-x-system-design]] for which algorithms/data structures sit underneath these
patterns.

---
**Related:** [[home]] · [[databases]] · [[distributed-systems]] · [[dsa-x-system-design]]
