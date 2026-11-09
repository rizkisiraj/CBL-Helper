---
type: concept
status: answered
tags: [database, system-design, bridge]
---

# Cache (Redis)

A fast in-memory layer between app and DB (reads under 1ms vs 5-50ms for the DB). Use when reads far exceed writes, or many users hit the same expensive computation. Caveat: many teams cache to mask problems that actually need better indexing — fix the query first, cache second.

## Under the hood (DSA)
- Under the hood it's a [[hashmap]] with eviction policies and expiry added on top.
- Its default eviction policy is the same one implemented in an [[lru-cache]]: evict whatever was least recently used.
- The memory it spends to buy speed is the same tradeoff a DSA solution makes when it allocates an auxiliary structure — see [[space-complexity]].

**Answers:** [[what-is-a-cache-redis]]
**Sources:** [[bytebytego]]
**Related:** [[cache-hit-ratio]] · [[message-queue]]
