---
type: dsa-session
date: 2026-04-15
tags: [dsa, greedy, kadanes-algorithm]
problems: [53]
patterns: ["[[greedy]]", "[[kadanes-algorithm]]"]
---

# 📘 Daily DSA Session Notes
**Date**: 2026-04-15
**Problem**: 53. Maximum Subarray
**Topic / Pattern**: Greedy / Kadane's Algorithm

---

## Problem Summary
Given an integer array, find the contiguous subarray with the largest sum and return that sum.

---

## Approach

**Core Idea**
At each index, decide: extend the current subarray or start a new one from here. Track the running best.

**Why It Works**
- If `currentSum + nums[i]` is worse than starting fresh at `nums[i]`, the previous subarray is a liability — cut it
- `maxSum` always holds the best seen so far, so we never lose a peak
- Initializing to `nums[0]` (not `0`) handles all-negative arrays correctly

---

## Final Solution (Code)

```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        currentSum = nums[0]
        maxSum = nums[0]

        for i in range(1, len(nums)):
            currentSum = max(nums[i], currentSum + nums[i])
            maxSum = max(maxSum, currentSum)

        return maxSum
```

---

## Review Summary

|                  |                                             |
| ---------------- | ------------------------------------------- |
| ✅ What worked    | Kadane's core logic correct; initialized to `nums[0]` not `0` |
| ⚠️ Watch out for | Redundant guard for `len == 1`; debug `print` left in; initial wrong space complexity guess |
| 🧠 Pattern       | [[kadanes-algorithm|Kadane's Algorithm]] ([[greedy|Greedy]] / Sliding Window variant) |
| ⏱️ Complexity    | Time: O(n) · Space: O(1) |

---

## Key Learnings

- Initializing to `nums[0]` instead of `0` is critical — `0` breaks all-negative inputs
- `range(1, n)` on a 1-element list is empty — no guard needed for that case
- Dead code (redundant guards) adds noise; delete it
- Space complexity = variables you *store*, not the input size
