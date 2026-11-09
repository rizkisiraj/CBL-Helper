---
type: dsa-session
date: 2026-04-25
tags: [dsa, sliding-window, hashmap]
problems: [3]
patterns: ["[[sliding-window]]", "[[hashmap]]"]
---

# 📘 Daily DSA Session Notes
**Date**: 2026-04-25
**Problem**: 3. Longest Substring Without Repeating Characters
**Topic / Pattern**: Sliding Window + HashMap

---

## Problem Summary
Given a string, find the length of the longest substring that contains no repeating characters.

---

## Approach

**Core Idea**
Maintain a sliding window `[left_indicator, index]`. Use a dict to store the last seen index of each character. When a duplicate is found, jump `left_indicator` forward past the previous occurrence.

**Why It Works**
- The dict gives O(1) lookup of where a duplicate last appeared
- `max(left_indicator, word_dict[char] + 1)` ensures the left pointer never moves backward (critical for correctness)
- Window size at each step is `index - left_indicator + 1`

---

## Final Solution

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        word_dict = {}
        left_indicator = 0
        longest = 0

        for index, char in enumerate(s):
            if char in word_dict:
                left_indicator = max(left_indicator, word_dict[char] + 1)

            word_dict[char] = index
            longest = max(longest, index - left_indicator + 1)

        return longest
```

---

## Review Summary

|                  |                                                                 |
| ---------------- | --------------------------------------------------------------- |
| ✅ What worked    | Correct sliding window intuition from the start; O(n) amortized reasoning was right when challenged; clean refactor to standard form |
| ⚠️ Watch out for | Variable shadowing in inner loops — worked by accident, not design. Always name inner loop variables differently. |
| 🧠 Pattern       | [[sliding-window|Sliding Window]] + [[hashmap|HashMap]] (track last seen index)                |
| ⏱️ Complexity    | Time: O(n) — Space: O(n)                                        |

---

## Key Learnings

- `max(left, dict[char] + 1)` is the standard sliding window move — the `max` prevents the window from shrinking backwards when a stale duplicate exists in the dict
- Window size = `index - left + 1`, not `len(dict)` — your original approach using `len(word_dict)` also worked but breaks down conceptually if a char's dict entry is stale
- Amortized O(n): each character is touched at most twice (once added, once evicted) — total work is O(n) regardless of inner loop structure
- Variable shadowing in Python doesn't throw errors — it silently does something you didn't intend
