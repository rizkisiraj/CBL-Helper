---
type: concept
status: open
tags: [dsa, system-design, bridge]
---

# LRU Cache

A fixed-capacity cache that evicts the **least recently used** entry when full. Built
from a [[hashmap]] (key → node, for O(1) lookup) plus a doubly linked list (tracks
recency order, so the least-recently-used node is always at the tail and any accessed
node can move to the head in O(1)). Neither structure alone is enough: a hashmap has no
notion of order, and a plain linked list has no O(1) lookup by key.

**Swift skeleton:**
```swift
final class Node {
    let key: Int
    var value: Int
    var prev: Node?
    var next: Node?
    init(_ key: Int, _ value: Int) { self.key = key; self.value = value }
}

final class LRUCache {
    private let capacity: Int
    private var map: [Int: Node] = [:]
    private let head = Node(-1, -1)
    private let tail = Node(-1, -1)

    init(_ capacity: Int) {
        self.capacity = capacity
        head.next = tail
        tail.prev = head
    }
    // move-to-front on get, evict tail.prev on put when over capacity
}
```

## In real systems
- This is exactly the eviction policy [[cache-redis|Redis]] and CDN caches use by default to decide what to drop when memory is full.

**Related:** [[hashmap]] · [[cache-redis]] · [[cache-hit-ratio]]
