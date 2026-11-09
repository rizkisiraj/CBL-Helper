---
type: concept
status: open
tags: [dsa, bridge]
---

# Binary Search

Repeatedly halve a sorted search space: compare the midpoint to the target, then
discard the half that can't contain it. Each step cuts the remaining space in half, so
`n` elements take only `log2(n)` steps instead of `n`.

**When to recognize it:** the data is sorted (or can be framed as sorted), and the
question is "find this value" or "find the boundary where a condition flips from false
to true" — not just scan everything.

**Swift skeleton:**
```swift
func search(_ nums: [Int], _ target: Int) -> Int {
    var left = 0
    var right = nums.count - 1

    while left <= right {
        let mid = left + (right - left) / 2
        if nums[mid] == target { return mid }
        if nums[mid] < target { left = mid + 1 } else { right = mid - 1 }
    }
    return -1
}
```

## In real systems
- A [[b-tree-index|B-tree]] index lookup is the same "halve the range" logic generalized to disk pages instead of an in-memory array — that's why an indexed lookup is O(log n).
- [[consistent-hashing]] uses binary search over sorted node positions on the hash ring to find the next node clockwise from a key.

**Related:** [[two-pointers]] · [[b-tree-index]]
