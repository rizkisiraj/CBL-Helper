---
type: concept
status: open
tags: [distributed-systems, bridge]
---

# Consistent Hashing

Map both nodes and keys onto the same hash ring (a circular space of hash values);
each key belongs to the first node clockwise from its position. Adding or removing a
node only remaps the keys between it and its neighbor — not the whole dataset, unlike
plain `hash(key) % n` where changing `n` reshuffles almost everything. This is what
lets a distributed store scale without a full data re-shuffle every time a node joins
or leaves.

## Under the hood (DSA)
- Node positions on the ring are a [[hashmap]] from hash value → node.
- Finding "the first node clockwise from this key" over sorted node positions is a [[binary-search]].
- This is the mechanism behind how [[cassandra-distributed-architecture|Cassandra]] assigns partitions to nodes on its ring.

**Related:** [[hashmap]] · [[binary-search]] · [[cassandra-distributed-architecture]]
