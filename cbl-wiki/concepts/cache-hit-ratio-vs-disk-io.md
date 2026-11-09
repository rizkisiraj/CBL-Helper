---
type: concept
status: answered
tags: [database, system-design]
---

# Cache hit ratio vs disk I/O

Inversely related: a high hit ratio (99%+) means almost all reads come from RAM and disk I/O stays low; a dropping ratio means more misses fall through to disk and I/O spikes. When disk I/O climbs on your dashboard, check cache hit ratio first.

**Answers:** [[cache-hit-ratio-disk-io-relationship]]
**Sources:** [[postgresql-docs]]
**Related:** [[cache-hit-ratio]]
