---
type: log
tags: [database, system-design]
challenge: db-stress-testing
---

# Wiki Log

- 2026-06-23 — Backfilled wiki from `cbl-session.md`: challenge, 52 question pages, 8 source pages, 21 concept pages (21 questions answered). Replaces the Miro mirror.
- 2026-06-23 — Ingested `raw/discord-database-article.md` → source [[discord-database-transformation]]; created concepts [[discord-database-evolution]], [[cassandra-distributed-architecture]], [[scylladb-vs-cassandra]], [[cql-query-language]]; answered [[horizontal-vs-vertical-scaling]] (22/52).
- 2026-06-23 — Logged learning: horizontal-vs-vertical-scaling. Session file reconciled (question checked off, finding appended). Wiki already up to date from ingest.
- 2026-06-23 — Ingested `raw/Martin-Kleppmann---Designing-Data-Intensive-Applications…(2017).pdf` (selective: Ch.1 performance, Ch.1 scaling, Ch.3 storage). Upgraded source [[designing-data-intensive-applications]] (added raw_file); created concepts [[latency-vs-throughput]], [[tail-latency-amplification]], [[slo-sla-targets]], [[database-index]], [[b-tree-index]], [[scaling-up-vs-out]]; extended [[latency-percentiles]]; answered [[latency-vs-throughput]], [[performance-good-enough-targets]], [[what-is-an-index]] (25/52).
- 2026-06-24 — Lint pass: no broken links / no orphans / 25-27-52 counts consistent. **Flagged** slug collision `latency-vs-throughput` (exists as both `questions/` and `concepts/` — ambiguous in Obsidian); proposed renaming the question to `latency-vs-throughput-tradeoff` (pending). Noted 8 backfilled concepts with no cited source (offered web enrichment, pending consent). No contradictions or near-duplicates found.
- 2026-06-24 — Lint follow-up (applied). **Renamed** question `latency-vs-throughput` → `latency-vs-throughput-tradeoff` to resolve the slug collision with the concept; repointed the question-targeting links in [[index]] and concept [[latency-vs-throughput]] (`**Answers:**`). **Enrichment** (web): created 4 source pages [[prometheus-docs]], [[grafana-docs]], [[pgbouncer-docs]], [[aws-rds-monitoring]]; added `**Sources:**` backlinks to all 8 previously-uncited concepts ([[prometheus]], [[grafana]], [[pgbouncer]], [[managed-db-monitoring-gaps]] → new pages; [[cache-hit-ratio]], [[cache-hit-ratio-vs-disk-io]] → [[postgresql-docs]]; [[tps]] → [[pgbench-tutorial]]; [[database-bottleneck]] → [[designing-data-intensive-applications]]); extended those existing sources' `**Taught:**` lists and added the 4 new sources to [[index]]. Sources now 9 → 13.
