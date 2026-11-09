---
type: hub
tags: [dsa, system-design, bridge]
---

# DSA × System Design

Where each DSA pattern shows up in a real system, and which algorithms power the
system design concepts already in this vault. See [[graph-views|Graph Views §6]] to
view this cluster in the Obsidian graph.

## Bridge table

| DSA concept | Where it shows up | System concept | One-line why |
|---|---|---|---|
| [[hashmap]] | In-memory key-value store | [[cache-redis]] | Redis is a hashmap with eviction and expiry bolted on |
| [[hashmap]] | Equality-only index | [[database-index]] | Hash indexes give O(1) equality lookup, no range queries (unlike a [[b-tree-index\|B-tree]]) |
| [[hashmap]] | Cache internals | [[lru-cache]] | HashMap + doubly linked list = O(1) get/put with eviction order |
| [[hashmap]] | Ring lookup | [[consistent-hashing]] | Node positions on the hash ring are stored in a hashmap |
| [[hashset]] | Idempotent consumers | [[message-queue]] | Dedup already-processed message IDs |
| [[sliding-window]] | Metric rate windows | [[tps]] | A rate is a count computed over a moving time window |
| [[sliding-window]] | Rate limiters | [[rate-limiting]] | Count requests in the last N seconds, not a fixed clock bucket |
| [[frequency-count]] | Hit/miss counting | [[cache-hit-ratio]] | Counting two outcomes is a frequency count |
| [[bucket-sort]] | Latency histograms | [[latency-percentiles]] | Bucket requests by time to estimate p95/p99 cheaply |
| [[prefix-suffix-products]] | Monitoring counters | [[tps]] | A rate is the difference of two cumulative running totals |
| [[two-pointers]] | LSM-tree compaction | [[lsm-tree]] / [[cassandra-distributed-architecture]] | Compaction is a sorted-file merge, same step as a two-pointer merge |
| [[two-pointers]] | Range scans / merge joins | [[b-tree-index]] | Walking two sorted inputs in lockstep |
| [[binary-search]] | Index lookups | [[b-tree-index]] | Root-to-leaf narrows the search range the same way binary search does |
| [[binary-search]] | Ring lookup | [[consistent-hashing]] | Find the next node clockwise over sorted ring positions |
| [[heap-priority-queue]] | Worker scheduling | [[message-queue]] | Process the most urgent job first, not just FIFO |
| [[heap-priority-queue]] | Streaming percentiles | [[latency-percentiles]] | Running median/percentile without re-sorting every request |
| [[dynamic-programming]] | Query planning | [[explain-vs-explain-analyze]] | Postgres's planner uses DP to choose join order |
| [[greedy]] | Load balancing | [[connection-pool-size]] | "Least connections" is a greedy per-request choice |
| [[space-complexity]] | Memory-for-speed tradeoff | [[database-index]] / [[cache-redis]] | Indexes and caches spend disk/RAM to buy lookup speed |

## Bridge problems to practice

LeetCode problems that are basically system design in miniature:

- **146. LRU Cache** → [[lru-cache]], [[cache-redis]] — not yet solved
- **359. Logger Rate Limiter** → [[rate-limiting]], [[sliding-window]] — not yet solved
- **295. Find Median from Data Stream** → [[heap-priority-queue]], [[latency-percentiles]] — not yet solved
- **23. Merge k Sorted Lists** → [[lsm-tree]], [[two-pointers]] — not yet solved
- **981. Time Based Key-Value Store** → [[binary-search]], [[hashmap]] — not yet solved
- **347. Top K Frequent Elements** → [[bucket-sort]], [[heap-priority-queue]] — already solved, see [[Day9]]

---
**Related:** [[home]] · [[dsa]] · [[system-design]] · [[distributed-systems]] · [[graph-views]]
