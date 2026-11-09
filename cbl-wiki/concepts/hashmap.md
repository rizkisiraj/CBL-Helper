---
type: concept
status: answered
tags: [dsa, bridge]
---

# HashMap

A hash-backed key→value store giving O(1) average-case lookup, insert, and update. The
general-purpose tool whenever you need to associate extra information with a value —
its count ([[frequency-count]]), its last-seen position ([[sliding-window]]), or
anything else beyond plain membership (that's [[hashset]]).

**When to recognize it:** the problem needs "for this key, what do I know about it" —
counts, indices, groupings — not just "have I seen it" (HashSet) and not bounded to a
small fixed alphabet (plain array).

**Swift skeleton:**
```swift
var counts: [Character: Int] = [:]
for char in "aab" {
    counts[char, default: 0] += 1
}
```

**Problems:**
- [[Day10]] — 3. Longest Substring Without Repeating Characters (maps char → last seen
  index)

## In real systems
- [[cache-redis|Redis]] is essentially a giant in-memory hashmap with eviction policies and key expiry bolted on.
- A database's hash index gives the same O(1) equality lookup as a hashmap, but — unlike a [[b-tree-index|B-tree]] — can't answer range queries; see [[database-index]].

**Related:** [[hashset]] · [[frequency-count]] · [[sliding-window]]
