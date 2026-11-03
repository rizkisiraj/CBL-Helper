---
type: concept
status: answered
tags: [cbl]
---

# Prometheus

Open-source monitoring that pulls metrics from HTTP endpoints every ~15s. For PostgreSQL, postgres_exporter connects to the DB and exposes metrics at :9187/metrics; Prometheus scrapes and stores time-series data, queried with PromQL.

**Answers:** [[what-is-prometheus]]
**Sources:** [[prometheus-docs]]
**Related:** [[grafana]]
