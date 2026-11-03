---
type: concept
status: answered
tags: [cbl]
---

# EXPLAIN vs EXPLAIN ANALYZE

EXPLAIN shows the planner's estimated plan without running the query (fast, safe). EXPLAIN ANALYZE actually executes it and reports real timing + row counts. Use EXPLAIN for quick checks and dangerous statements; use ANALYZE to diagnose real slowness — wrap destructive statements in BEGIN; ... ROLLBACK;.

**Answers:** [[explain-vs-analyze-when-to-use]]
**Sources:** [[postgresql-docs]]
**Related:** [[slow-query-log]]
