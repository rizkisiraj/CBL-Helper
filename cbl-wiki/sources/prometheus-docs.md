---
type: source
url: https://prometheus.io/docs/
tags: [database, system-design]
---

# Prometheus docs (+ postgres_exporter)

Official Prometheus documentation — a pull-based monitoring system that scrapes metrics
from HTTP endpoints and stores them as time series, queried with PromQL. For PostgreSQL,
the community **postgres_exporter** connects to the DB and exposes metrics on `:9187` for
Prometheus to scrape (CI-tested against PG 13–18, supports the multi-target pattern).

URL: https://prometheus.io/docs/
postgres_exporter: https://github.com/prometheus-community/postgres_exporter

**Taught:** [[prometheus]]
