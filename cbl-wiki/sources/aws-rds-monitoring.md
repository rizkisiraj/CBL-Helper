---
type: source
url: https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/MonitoringOverview.html
tags: [database, system-design]
---

# Amazon RDS monitoring docs

AWS documentation on monitoring managed PostgreSQL. **CloudWatch** collects instance
metrics (CPUUtilization, connections, IOPS) from the hypervisor at 1-minute resolution.
**Enhanced Monitoring** adds real OS-level metrics via an agent on the DB instance — the
visibility you otherwise lose on a managed DB (no SSH, no top/htop, no host extensions).
Illustrates the managed-vs-self-hosted monitoring tradeoff.

URL: https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/MonitoringOverview.html
CloudWatch metrics: https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/rds-metrics.html
Enhanced Monitoring: https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_Monitoring.OS.html

**Taught:** [[managed-db-monitoring-gaps]]
