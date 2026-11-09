---
type: dsa-session
date: 2026-04-10
tags: [dsa, greedy]
problems: [121]
patterns: ["[[greedy]]"]
---

# Day 3 — Session Notes
**Date**: 2026-04-10

---

## Problem: 121. Best Time to Buy and Sell Stock

**Approach**: Greedy — single pass, track the minimum buy price and compute max profit at each step.

### Final Solution (Swift)

```swift
class Solution {
    func maxProfit(_ prices: [Int]) -> Int {
        var currentBuy = prices[0]
        var profit = 0

        for value in prices {
            if currentBuy > value {
                currentBuy = value
            }
            profit = max(profit, value - currentBuy)
        }

        return profit
    }
}
```

---

## Review Summary

| | |
|---|---|
| ✅ What worked | Correct greedy logic, clean `max()` accumulation, good variable naming after fix |
| ⚠️ Watch out for | Variable names that mislead (`currentSell` → `currentBuy`); verbose init (`prices[0] - prices[0]` → `0`) |
| 🧠 Pattern | [[greedy|Greedy]] (not [[kadanes-algorithm|Kadane's]]) — one pass, locally optimal choice at each step |
| ⏱️ Complexity | O(n) time, O(1) space |

---

## Key Learnings

- **Pattern**: Greedy minimum tracking — update min price when cheaper found, record best profit at each step.
- **Space complexity intuition**: Count extra variables — if the count doesn't grow with input size, it's O(1).
- **Kadane's vs Greedy**: Kadane's solves max subarray *sum*. This problem is a direct greedy approach (though it can be reduced to Kadane's via a difference array).
- **Code clarity**: `var profit = 0` is cleaner than `prices[0] - prices[0]`.

---
