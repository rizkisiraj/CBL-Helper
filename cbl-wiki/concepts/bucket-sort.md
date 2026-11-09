---
type: concept
status: answered
tags: [dsa]
---

# Bucket Sort

Sort by using a bounded, known quantity (frequency, digit value) directly as an array
index, instead of comparing elements against each other. Since frequency of any element
in an array of size `n` can never exceed `n`, a bucket array of size `n+1` indexed by
frequency always fits — then you walk the buckets from high to low to get results in
descending frequency order, with no comparison sort at all.

**When to recognize it:** "top K frequent," or any problem where the sort key is
bounded by the input size rather than arbitrary — that bound is what makes bucketing
cheaper than a comparison sort (O(n) vs O(n log n)).

**Swift skeleton:**
```swift
func topKFrequent(_ nums: [Int], _ k: Int) -> [Int] {
    var freq = [Int: Int]()
    for n in nums { freq[n, default: 0] += 1 }

    var buckets = Array(repeating: [Int](), count: nums.count + 1)
    for (num, count) in freq { buckets[count].append(num) }

    var result = [Int]()
    for count in stride(from: nums.count, through: 0, by: -1) {
        for num in buckets[count] where result.count < k {
            result.append(num)
        }
        if result.count >= k { break }
    }
    return result
}
```

**Problems:**
- [[Day9]] — 347. Top K Frequent Elements

**Related:** [[frequency-count]]
