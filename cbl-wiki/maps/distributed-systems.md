---
type: hub
tags: [distributed-systems]
---

# Distributed Systems

All `distributed-systems`-tagged concepts and questions — what changes once data and
load are spread across many machines instead of one.

## Concepts
- [[scaling-up-vs-out]] — vertical vs horizontal scaling, and why "keep it on one node
  until forced out" is the default
- [[cassandra-distributed-architecture]] — distributed hash ring, nodes, replication,
  gossip protocol
- [[scylladb-vs-cassandra]] — same model, C++ rewrite for no GC pauses
- [[cql-query-language]] — Cassandra's query language
- [[discord-database-evolution]] — MongoDB → Cassandra → ScyllaDB case study
- [[tail-latency-amplification]] — why one slow node slows an entire fanned-out request
- [[lsm-tree]] — write-optimized storage behind Cassandra's memtable/SSTable flow
- [[consistent-hashing]] — how keys get assigned to nodes on the ring without a full
  reshuffle on every join/leave

## Open/answered questions
- Horizontal vs vertical scaling — when do you hit the limits of a bigger server?
- What is the CAP theorem in simple terms — what tradeoffs exist when scaling?

(See `challenges/db-stress-testing/questions/horizontal-vs-vertical-scaling.md` and
`cap-theorem.md` for status.)

---
**Related:** [[home]] · [[databases]] · [[system-design]]
