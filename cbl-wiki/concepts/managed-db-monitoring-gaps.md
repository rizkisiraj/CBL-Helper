---
type: concept
status: answered
tags: [database, system-design]
---

# Managed DB monitoring gaps

Managed databases (RDS, Cloud SQL) cost you OS-level visibility: no raw disk I/O detail, no top/htop, no filesystem access, no arbitrary host extensions/exporters, no kernel tuning. AWS/GCP compensate with CloudWatch / Cloud Monitoring covering the key metrics. Usually worth the tradeoff.

**Answers:** [[managed-db-monitoring-limits]]
**Sources:** [[aws-rds-monitoring]]
**Related:** [[prometheus]]
