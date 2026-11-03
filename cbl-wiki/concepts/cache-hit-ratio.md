---
type: concept
status: answered
tags: [cbl]
---

# Cache hit ratio

Percentage of reads served from RAM vs slow disk: hits / (hits + misses). Target 99%+. Below 90% means working data likely exceeds RAM. PostgreSQL: set shared_buffers to ~25% of system RAM.

**Answers:** [[what-is-cache-hit-ratio]]
**Sources:** [[postgresql-docs]]
**Related:** [[cache-hit-ratio-vs-disk-io]] · [[cache-redis]]
