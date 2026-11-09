---
type: dsa-session
date: 2026-04-17
tags: [dsa, dynamic-programming]
problems: [152]
patterns: ["[[dynamic-programming]]"]
---

# 📘 Daily DSA Session Notes
**Date**: 2026-04-17
**Problem**: 152. Maximum Product Subarray
**Topic / Pattern**: Dynamic Programming (tracking curMax + curMin)

---

## Problem Summary
Given an integer array, find the contiguous subarray that has the largest product and return that product.

---

## Approach

**Core Idea**
At each index, track both the maximum and minimum product ending at that position. The minimum matters because a very negative value can flip into the largest positive when multiplied by another negative number.

**Why It Works**
- A single running product (like Max Subarray) fails here because negatives can "flip" the sign
- At each step, the new `curMax` is the best of: starting fresh, extending the max streak, or flipping the min streak
- Same logic applies to `curMin`
- **Critical**: save `tempMax = curMax` before updating, because `curMin` needs the old `curMax` — not the newly computed one

---

## Final Solution (Code)

```swift
class Solution {
    func maxProduct(_ nums: [Int]) -> Int {
        var currentMax = nums[0]
        var currentMin = nums[0]
        var maxProduct = nums[0]

        for index in 1..<nums.count {
            let tempMax = currentMax
            currentMax = max(nums[index], currentMax * nums[index], currentMin * nums[index])
            currentMin = min(nums[index], tempMax * nums[index], currentMin * nums[index])
            maxProduct = max(maxProduct, currentMax)
        }

        return maxProduct
    }
}
```

---

## Review Summary

|                  |                                             |
| ---------------- | ------------------------------------------- |
| ✅ What worked    | Correctly tracked both `curMax` and `curMin`; caught the `tempMax` bug after prompting and fixed it cleanly |
| ⚠️ Watch out for | Using the updated `curMax` when computing `curMin` — always save `tempMax` before the update step |
| 🧠 Pattern       | [[dynamic-programming|DP]] — optimal substructure at each index; similar structure to [[kadanes-algorithm|Kadane's]] but tracking two states instead of one |
| ⏱️ Complexity    | Time: O(n) · Space: O(1) |
| 🔁 Next step     | Trace through `[2, 0, 3]` and `[-1, -2, -3]` to verify edge case handling |

---

## Key Learnings

- Unlike Max Subarray, you must track **both** curMax and curMin — negatives make the minimum a future asset
- Always preserve the old `curMax` in a temp variable before overwriting — stale value bug is subtle and easy to miss
- Initialized all three variables to `nums[0]` (not `0`) — same reason as Kadane's: handles all-negative inputs
- The three candidates at each step: start fresh, extend max streak, flip min streak
       