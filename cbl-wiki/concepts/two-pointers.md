---
type: concept
status: answered
tags: [dsa]
---

# Two Pointers

Move two indices toward each other (or in the same direction at different speeds) to
compare or combine elements without nested loops. For "converging from both ends,"
`left < right` is the canonical exit condition — it stops exactly at the midpoint with
no wasted iterations and needs no separate empty-string guard.

**When to recognize it:** palindrome checks, sorted-array pair-sum problems, or any
"compare the outside working inward" pattern. Cleaning/normalizing the input first
(e.g. stripping non-alphanumeric characters) often simplifies the comparison loop, at
the cost of O(n) extra space — doing the skip in-place during the pointer walk trades
that space back for a slightly more complex loop.

**Swift skeleton:**
```swift
func isPalindrome(_ s: String) -> Bool {
    let chars = Array(s.lowercased().filter { $0.isLetter || $0.isNumber })
    var left = 0
    var right = chars.count - 1

    while left < right {
        if chars[left] != chars[right] { return false }
        left += 1
        right -= 1
    }
    return true
}
```

**Problems:**
- [[Day12]] — 125. Valid Palindrome

**Related:** [[stack]] · [[sliding-window]]
