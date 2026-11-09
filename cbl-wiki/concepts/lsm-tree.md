---
type: concept
status: open
tags: [database, distributed-systems, bridge]
---

# LSM-Tree (log-structured merge-tree)

A write-optimized storage structure: writes go to an in-memory **memtable** and are
only appended, never edited in place. Once the memtable fills, it's flushed to disk as
an immutable sorted file (an SSTable). Reads check the memtable, then the on-disk files
newest-first. Periodically, **compaction** merges multiple sorted files into one,
discarding overwritten/deleted keys. Contrast with a [[b-tree-index|B-tree]], which
updates in place — LSM trades read simplicity for much cheaper writes (append instead
of random-access overwrite).

## Under the hood (DSA)
- Compaction merges several already-sorted files into one, the same operation as a [[two-pointers|two-pointer]] merge over sorted arrays — just generalized to more than two inputs (a k-way merge).

**Related:** [[two-pointers]] · [[cassandra-distributed-architecture]] · [[b-tree-index]]
