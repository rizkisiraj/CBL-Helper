---
type: concept
status: answered
tags: [database, bridge]
---

# EXPLAIN vs EXPLAIN ANALYZE

EXPLAIN shows the planner's estimated plan without running the query (fast, safe). EXPLAIN ANALYZE actually executes it and reports real timing + row counts. Use EXPLAIN for quick checks and dangerous statements; use ANALYZE to diagnose real slowness — wrap destructive statements in BEGIN; ... ROLLBACK;.

## Under the hood (DSA)
- The plan shown by EXPLAIN comes from the planner choosing join order with [[dynamic-programming]] — overlapping sub-plans, built up the same way [[kadanes-algorithm]]/DP solutions track running state — switching to a genetic optimizer once the table count makes exhaustive DP too slow.

**Answers:** [[explain-vs-analyze-when-to-use]]
**Sources:** [[postgresql-docs]]
**Related:** [[slow-query-log]]
