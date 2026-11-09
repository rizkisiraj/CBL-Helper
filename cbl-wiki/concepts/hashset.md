---
type: concept
status: answered
tags: [dsa, bridge]
---

# HashSet

A hash-backed set gives O(1) average-case membership checks and O(1) insertion. Use it
when the question is "have I seen this before?" rather than "how many times?" (that's
[[frequency-count]] instead).

**When to recognize it:** duplicate detection, "does this value already exist,"
deduplication, or any single-pass problem where you only need yes/no membership — not a
count. In Swift, `Set<T>.insert(_:)` returns `(inserted: Bool, memberAfterInsert: T)`,
so you can check-and-insert in one call instead of `contains` + `insert`.

**Swift skeleton:**
```swift
func containsDuplicate(_ nums: [Int]) -> Bool {
    var seen = Set<Int>()
    for value in nums {
        if !seen.insert(value).inserted {
            return true
        }
    }
    return false
}
```

**Problems:**
- [[Day4]] — 217. Contains Duplicate (single-pass `insert(_:).inserted` check)

## In real systems
- Idempotent message consumers deduplicate by keeping a set of already-processed message IDs — see [[message-queue]].

**Related:** [[frequency-count]] · [[hashmap]]
