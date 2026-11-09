---
type: concept
status: answered
tags: [database, system-design]
---

# Cache (Redis)

A fast in-memory layer between app and DB (reads under 1ms vs 5-50ms for the DB). Use when reads far exceed writes, or many users hit the same expensive computation. Caveat: many teams cache to mask problems that actually need better indexing — fix the query first, cache second.

**Answers:** [[what-is-a-cache-redis]]
**Sources:** [[bytebytego]]
**Related:** [[cache-hit-ratio]] · [[message-queue]]
