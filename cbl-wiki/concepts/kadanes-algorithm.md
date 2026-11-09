---
type: concept
status: answered
tags: [dsa]
---

# Kadane's Algorithm

A specific [[greedy]] recipe for "best contiguous subarray" problems: at each index,
decide whether to extend the running subarray or abandon it and start fresh at the
current element, then track the best value seen across all steps.

**When to recognize it:** "contiguous subarray" + "maximum/minimum sum or product."
The signature move is `running = max(nums[i], running + nums[i])` (sum variant) — if
extending makes things worse than starting over, cut the running streak loose.
Initialize to `nums[0]`, not `0`, so all-negative inputs are handled correctly.

**Swift skeleton:**
```swift
func maxSubArray(_ nums: [Int]) -> Int {
    var currentSum = nums[0]
    var maxSum = nums[0]

    for value in nums.dropFirst() {
        currentSum = max(value, currentSum + value)
        maxSum = max(maxSum, currentSum)
    }

    return maxSum
}
```

**Problems:**
- [[Day6]] — 53. Maximum Subarray (the canonical sum version)
- [[Day7]] — 152. Maximum Product Subarray (tracks both a max *and* a min streak, since
  a negative can flip sign — see [[dynamic-programming]])

**Related:** [[greedy]] · [[dynamic-programming]]
