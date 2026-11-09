---
type: concept
status: answered
tags: [database, system-design, bridge]
---

# Cache hit ratio

Percentage of reads served from RAM vs slow disk: hits / (hits + misses). Target 99%+. Below 90% means working data likely exceeds RAM. PostgreSQL: set shared_buffers to ~25% of system RAM.

## Under the hood (DSA)
- It's a [[frequency-count]] of two outcomes (hit vs miss) — the same tally as counting character frequencies, just over cache accesses.

**Answers:** [[what-is-cache-hit-ratio]]
**Sources:** [[postgresql-docs]]
**Related:** [[cache-hit-ratio-vs-disk-io]] · [[cache-redis]]
