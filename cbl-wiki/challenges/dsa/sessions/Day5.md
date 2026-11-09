---
type: dsa-session
date: 2026-04-15
tags: [dsa, prefix-suffix-products]
problems: [238]
patterns: ["[[prefix-suffix-products]]"]
---

# 📘 Daily DSA Session Notes
**Date**: 2026-04-15
**Problem**: 238. Product of Array Except Self
**Topic / Pattern**: Prefix/Suffix Products

---

## Problem Summary
Given an integer array `nums`, return an array where `output[i]` is the product of all elements in `nums` except `nums[i]`. Must run in O(n) time and without using the division operator.

---

## Approach

**Core Idea**
Two-pass technique: first pass builds prefix products directly into the output array, second pass multiplies in the suffix product using a single running variable.

**Why It Works**
- `output[i] = (product of all elements left of i) × (product of all elements right of i)`
- Splitting into prefix and suffix avoids division entirely
- A running variable replaces a full suffix array — only the accumulated product is ever needed at each step
- Handles zeros naturally since there's no division involved

---

## Final Solution (Code)

```swift
class Solution {
    func productExceptSelf(_ nums: [Int]) -> [Int] {
        var prod = Array(repeating: 1, count: nums.count)
        var runningVariable = 1

        for index in 1..<nums.count {
            prod[index] = prod[index-1] * nums[index-1]
        }

        for index in stride(from: nums.count-2, through: 0, by: -1) {
            runningVariable *= nums[index+1]
            prod[index] *= runningVariable
        }

        return prod
    }
}
```

---

## Review Summary

|                  |                                             |
| ---------------- | ------------------------------------------- |
| ✅ What worked    | Correct prefix logic from the start; independently identified that `suffixProd` array was wasteful; zeros handled correctly |
| ⚠️ Watch out for | Ask "do I need this whole array or just a running value?" before allocating auxiliary arrays |
| 🧠 Pattern       | [[prefix-suffix-products|Prefix/Suffix Products]] (Arrays)             |
| ⏱️ Complexity    | Time: O(n) — two linear passes <br> Space: O(1) extra (output array excluded by convention) |

---

## Key Learnings

* Prefix × suffix decomposition avoids division and handles zeros naturally
* A running variable can replace a full auxiliary array when you only need the accumulated product — always ask this before allocating
* Output array doesn't count toward space complexity by convention
* First instinct was O(n) space — recognizing the optimization to O(1) is the key follow-up interviewers look for on this problem
