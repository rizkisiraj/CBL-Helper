---
type: dsa-session
date: 2026-04-11
tags: [dsa, hashset]
problems: [217]
patterns: ["[[hashset]]"]
---

# Day 4 — Session Notes
**Date**: 2026-04-11

---

## Problem: 217. Contains Duplicate

**Approach**: HashSet — single pass, insert each value and check if it was already present using `insert(_:).inserted`.

### Final Solution (Swift)

```swift
func containsDuplicate(_ nums: [Int]) -> Bool {
    var numbersSet = Set<Int>()

    for value in nums {
        if !numbersSet.insert(value).inserted {
            return true
        }
    }

    return false
}
```

---

## Review Summary

| | |
|---|---|
| ✅ What worked | Correct Set usage, early return on first duplicate, clean loop structure |
| ⚠️ Watch out for | Calling `contains` + `insert` separately when `insert` return value already tells you if the element existed |
| 🧠 Pattern | [[hashset|HashSet]] for O(1) membership tracking — single pass |
| ⏱️ Complexity | O(n) time, O(n) space |

---

## Key Learnings

- **`Set.insert(_:)` return value**: Returns `(inserted: Bool, memberAfterInsert: T)` — use `.inserted` to check if the element was new, eliminating a separate `contains` call.
- **Empty array edge case**: Loop never runs → returns `false` correctly. Always trace through edge cases before submitting.
- **Pattern**: HashSet single-pass is the foundation for many hashing problems (e.g., Two Sum, Group Anagrams). Recognize it early.
- **Swift idiom**: `!numbersSet.insert(value).inserted` is more idiomatic than a separate `contains` check.

---
