---
type: dsa-session
date: 2026-04-24
tags: [dsa, bucket-sort]
problems: [347]
patterns: ["[[bucket-sort]]"]
---

# 📘 Daily DSA Session Notes
**Date**: 2026-04-24
**Problem**: 347. Top K Frequent Elements
**Topic / Pattern**: Bucket Sort / Counting

---

## Problem Summary
Given an array of integers and a number k, return the k most frequently occurring elements.

---

## Approach

**Core Idea**
Use frequency as an array index (bucket sort). Since max frequency ≤ n, create a bucket array of size n+1 where index = frequency. Walk from highest to lowest index collecting elements until you have k.

**Why It Works**
- Avoids sorting entirely — no O(n log n) overhead
- Max possible frequency is bounded by n, so bucket size is fixed
- Walking high-to-low guarantees we pick the most frequent first

---

## Final Solution (Code)

```swift
class Solution {
    func topKFrequent(_ nums: [Int], _ k: Int) -> [Int] {
        var numerosDict = [Int:Int]()
        var bucketArray = Array(repeating: [Int](), count: nums.count + 1)
        var result = [Int]()

        for value in nums {
            numerosDict[value, default: 0] += 1
        }

        for (num, freq) in numerosDict {
            bucketArray[freq].append(num)
        }

        for index in stride(from: nums.count, to: 0, by: -1) {
            if bucketArray[index].isEmpty { continue }
            for num in bucketArray[index] {
                if result.count < k {
                    result.append(num)
                } else {
                    break
                }
            }
            if result.count >= k { break }
        }

        return result
    }
}
```

---

## Review Summary

|                  |                                             |
| ---------------- | ------------------------------------------- |
| ✅ What worked    | - Bucket sort insight (O(n) vs heap O(n log n)) <br> - Correct bucket sizing (n+1) <br> - Clean Swift dictionary idiom |
| ⚠️ Watch out for | - `break` only exits the inner loop — always check which loop your break targets |
| 🧠 Pattern       | [[bucket-sort|Bucket Sort]] / Counting Sort                  |
| ⏱️ Complexity    | Time: O(n) <br> Space: O(n)                 |

---

## Key Learnings

- Bucket sort is O(n) because frequency is a bounded index, not a comparison sort
- Always ask: does my `break` exit the right loop?
- Heap approach (`topK` with a max-heap) is O(n log k) — worse than this for large n
