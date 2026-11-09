---
type: concept
status: answered
tags: [database]
---

# B-tree index (the default relational index)

The standard index implementation in relational databases (PostgreSQL included), in use since 1970.

- Keeps key→value pairs **sorted by key**, enabling both exact lookups and **range queries**.
- Breaks storage into **fixed-size pages** (traditionally ~4 kB), matching how disks are arranged. Pages reference child pages (on-disk pointers), forming a tree: start at the **root**, follow the page whose key range contains your key, down to a **leaf page** holding the value.
- A node holds *k* keys + *k+1* child references, with *k* often in the hundreds — so the tree is wide and shallow. With *n* keys the height is **O(log n)**, so even a huge table needs only a few page reads per lookup; this is the structure behind the [[database-index|index]]'s O(log n) speedup.
- **Writes happen in place:** update = overwrite the leaf page; insert into a full page **splits** it into two half-full pages and updates the parent. (Contrast: log-structured / LSM-tree indexes only append.) In-place page overwrites are why a crash mid-split can corrupt an index, motivating a write-ahead log.

**Sources:** [[designing-data-intensive-applications]]
**Related:** [[database-index]] · [[spot-a-seq-scan]]
