---
type: source
url: https://www.pgbouncer.org/
tags: [database, system-design]
---

# PgBouncer docs

Official PgBouncer documentation. A lightweight connection pooler that clients connect to
as if it were Postgres; it reuses a small set of real backend connections instead of
opening one per client. Three pooling modes — **session**, **transaction** (best for web
apps: a backend is held only for the duration of a transaction), and **statement**.

URL: https://www.pgbouncer.org/features.html
Config: https://www.pgbouncer.org/config.html

**Taught:** [[pgbouncer]]
