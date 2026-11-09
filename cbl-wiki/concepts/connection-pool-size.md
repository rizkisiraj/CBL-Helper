---
type: concept
status: answered
tags: [database, system-design, bridge]
---

# Connection pool size

No universal formula, but a known starting point: pool_size = (core_count × 2) + effective_spindle_count. Counterintuitively, more connections does not mean more performance — beyond the optimal point, context-switching overhead kills throughput. Load test to find the sweet spot.

## Under the hood (DSA)
- "Least connections" load balancing — send each request to whichever backend has the fewest open connections right now, never revisiting the choice — is a [[greedy]] algorithm applied per request.

**Answers:** [[connection-pool-size-formula]]
**Sources:** [[designing-data-intensive-applications]]
**Related:** [[pgbouncer]]
