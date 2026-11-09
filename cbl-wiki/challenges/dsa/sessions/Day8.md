---
type: dsa-session
date: 2026-04-23
tags: [dsa, frequency-count]
problems: [242]
patterns: ["[[frequency-count]]"]
---

# 📘 Daily DSA Session Notes
**Date**: 2026-04-23
**Problem**: 242. Valid Anagram
**Topic / Pattern**: Frequency Count (fixed-size array as HashMap)

---

## Problem Summary
Given two strings `s` and `t`, return `true` if `t` is an anagram of `s` — meaning both strings contain the same characters with the same frequencies.

---

## Approach

**Core Idea**
Count character frequencies for both strings using a fixed-size array of 26 integers (one per lowercase letter), then compare.

**Why It Works**
- Two strings are anagrams iff their character frequency distributions are identical
- ASCII offset `char - 'a'` maps each letter to index 0–25
- Early length check avoids unnecessary work

---

## Final Solution (Code)

```swift
class Solution {
    func isAnagram(_ s: String, _ t: String) -> Bool {
        if s.count != t.count { return false }

        var freqS = Array(repeating: 0, count: 26)
        var freqT = Array(repeating: 0, count: 26)

        for char in s {
            freqS[Int(char.asciiValue! - Character("a").asciiValue!)] += 1
        }
        for char in t {
            freqT[Int(char.asciiValue! - Character("a").asciiValue!)] += 1
        }

        return freqS == freqT
    }
}
```

---

## Review Summary

|                  |                                                              |
| ---------------- | ------------------------------------------------------------ |
| ✅ What worked    | Correct O(n) frequency approach; clean ASCII indexing; idiomatic array comparison |
| ⚠️ Watch out for | Force-unwrap on `asciiValue!` breaks on non-lowercase/non-ASCII input |
| 🧠 Pattern       | [[frequency-count|Frequency Count]] — fixed-size array of 26 as a [[hashmap|HashMap]] substitute |
| ⏱️ Complexity    | Time: O(n) · Space: O(1) — array is always size 26 |

---

## Key Learnings

- Sorting approach works (O(n log n)) but frequency count is the optimal O(n) solution
- A fixed-size array of 26 is effectively O(1) space — always call that out explicitly
- Two strings of different lengths can never be equal — the early length check is a useful optimization, not just safety
- Next challenge: solve with a **single** frequency array — increment for `s`, decrement for `t`, return true if all zeros
