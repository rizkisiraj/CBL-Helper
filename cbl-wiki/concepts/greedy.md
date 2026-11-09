---
type: concept
status: answered
tags: [dsa, bridge]
---

# Greedy

Make the locally optimal choice at each step and never revisit it, trusting that the
sequence of local choices adds up to a globally optimal answer. No backtracking, no
exploring alternatives — one pass, one decision per step.

**When to recognize it:** the problem asks for a single running best (max profit, min
cost, max count) and a choice made at step `i` never needs to be undone once a later
step is seen. If you find yourself wanting to "go back and reconsider," it's probably
not greedy — it's DP or backtracking instead.

**Swift skeleton:**
```swift
func greedyScan(_ nums: [Int]) -> Int {
    var best = nums[0]
    var running = nums[0]

    for value in nums.dropFirst() {
        // update `running` with a locally optimal decision
        running = max(running, value)
        best = max(best, running)
    }

    return best
}
```

**Problems:**
- [[Day3]] — 121. Best Time to Buy and Sell Stock (track min-so-far, max profit at each step)
- [[Day6]] — 53. Maximum Subarray (Kadane's — see [[kadanes-algorithm]])

## In real systems
- "Least connections" load balancing picks whichever backend has the fewest open connections right now and never revisits that choice — a greedy decision made fresh per request, tied to [[connection-pool-size]].

**Related:** [[kadanes-algorithm]]
