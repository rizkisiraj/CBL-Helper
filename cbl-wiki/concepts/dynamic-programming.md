---
type: concept
status: answered
tags: [dsa, bridge]
---

# Dynamic Programming

Break a problem into overlapping subproblems and build the answer up from state carried
between steps, instead of exploring every possibility from scratch. The 1D "running
state" flavor looks a lot like [[greedy]]/[[kadanes-algorithm]], but tracks more than
one quantity when a single running value can't capture everything that matters.

**When to recognize it:** the problem has optimal substructure (the best answer at
index `i` is built from the best answer(s) at `i-1`), **and** a single greedy running
value isn't enough — e.g. a sign flip (negative × negative → positive) means the
*minimum* seen so far can become the *maximum* a step later, so you must track both.

**Swift skeleton:**
```swift
func maxProduct(_ nums: [Int]) -> Int {
    var currentMax = nums[0]
    var currentMin = nums[0]
    var best = nums[0]

    for value in nums.dropFirst() {
        let prevMax = currentMax
        currentMax = max(value, currentMax * value, currentMin * value)
        currentMin = min(value, prevMax * value, currentMin * value)
        best = max(best, currentMax)
    }

    return best
}
```
The critical bug to avoid: save the old `currentMax` in a temp before overwriting it —
`currentMin`'s update needs the *previous* max, not the one just computed.

**Problems:**
- [[Day7]] — 152. Maximum Product Subarray (tracks curMax + curMin per step)

## In real systems
- PostgreSQL's query planner uses dynamic programming to choose join order for a query — see [[explain-vs-explain-analyze]] — switching to a genetic optimizer once the table count gets too large for exhaustive DP.

**Related:** [[kadanes-algorithm]] · [[greedy]] · [[prefix-suffix-products]]
