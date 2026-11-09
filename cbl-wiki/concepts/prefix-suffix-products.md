---
type: concept
status: answered
tags: [dsa]
---

# Prefix/Suffix Products

Decompose "product of everything except index i" into `(product of everything left of
i) × (product of everything right of i)`. Build the prefix products in one left-to-right
pass, then fold in the suffix with a single running variable in a second, right-to-left
pass — no division operator needed, and zeros are handled naturally.

**When to recognize it:** "array except self" style problems where the forbidden move
is dividing by the excluded element (division breaks on zero and is often disallowed
outright). The key optimization: you don't need a full suffix *array* — a single
running product variable carries the same information in O(1) extra space.

**Swift skeleton:**
```swift
func productExceptSelf(_ nums: [Int]) -> [Int] {
    var output = Array(repeating: 1, count: nums.count)

    for i in 1..<nums.count {
        output[i] = output[i - 1] * nums[i - 1]
    }

    var suffix = 1
    for i in stride(from: nums.count - 2, through: 0, by: -1) {
        suffix *= nums[i + 1]
        output[i] *= suffix
    }

    return output
}
```

**Problems:**
- [[Day5]] — 238. Product of Array Except Self

**Related:** [[dynamic-programming]]
