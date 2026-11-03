---
type: concept
status: answered
tags: [cbl]
---

# Database index (what it is, and the read/write trade-off)

An **index is an additional structure derived from the primary data** — separate from the table's rows, maintained by the database, and addable/removable without changing the data itself.

- **Why it speeds reads:** scanning a table to find a key is **O(n)** — double the rows, double the lookup time. An index is a data structure (commonly a [[b-tree-index|B-tree]]) that turns that into roughly **O(log n)**: the DB jumps to the rows instead of reading every one. This is what replaces a full table scan (a [[spot-a-seq-scan|Seq Scan]]) with an index lookup.
- **The fundamental trade-off:** *well-chosen indexes speed up reads, but every index slows down writes* — each insert/update/delete must also update every index. So databases don't index everything by default; **you** choose indexes from the app's typical query patterns to get the most read benefit for the least write overhead.

**Answers:** [[what-is-an-index]]
**Sources:** [[designing-data-intensive-applications]]
**Related:** [[b-tree-index]] · [[spot-a-seq-scan]] · [[primary-foreign-keys]]
