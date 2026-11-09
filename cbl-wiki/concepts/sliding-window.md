---
type: concept
status: answered
tags: [dsa, bridge]
---

# Sliding Window

Maintain a window `[left, right]` over a sequence and slide its edges instead of
re-scanning from scratch at every position. Paired with a [[hashmap]] tracking "last
seen index," the window can jump `left` forward in O(1) the moment a duplicate is
found, rather than shrinking one step at a time.

**When to recognize it:** "longest/shortest substring or subarray satisfying some
constraint" — especially "no repeating characters," "at most K distinct," or similar.
The window only ever grows `right` one step and jumps `left` forward; it never moves
`left` backward (critical for O(n) amortized correctness).

**Swift skeleton:**
```swift
func lengthOfLongestSubstring(_ s: String) -> Int {
    var lastSeen = [Character: Int]()
    var left = 0
    var longest = 0

    for (index, char) in s.enumerated() {
        if let seenAt = lastSeen[char] {
            left = max(left, seenAt + 1)
        }
        lastSeen[char] = index
        longest = max(longest, index - left + 1)
    }
    return longest
}
```

**Problems:**
- [[Day10]] — 3. Longest Substring Without Repeating Characters

## In real systems
- A rate like [[tps]] is computed as a count over a moving time window (PromQL's `rate(...[5m])`), the same sliding-window idea applied to a metrics stream.
- A sliding-window [[rate-limiting|rate limiter]] counts requests in the last N seconds instead of resetting at fixed clock boundaries.

**Related:** [[hashmap]] · [[two-pointers]]
