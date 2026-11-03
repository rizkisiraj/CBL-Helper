---
type: concept
status: answered
tags: [cbl]
---

# Slow query log

Records every query exceeding a time threshold. PostgreSQL: log_min_duration_statement = 500 (ms). For aggregate analysis across all queries, use the pg_stat_statements extension instead.

**Answers:** [[what-is-a-slow-query-log]]
**Sources:** [[postgresql-docs]]
**Related:** [[explain-vs-explain-analyze]]
