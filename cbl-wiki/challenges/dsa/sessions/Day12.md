---
type: dsa-session
date: 2026-04-27
tags: [dsa, two-pointers]
problems: [125]
patterns: ["[[two-pointers]]"]
---

# 📘 Daily DSA Session Notes
**Date**: 2026-04-27
**Problem**: 125. Valid Palindrome
**Topic / Pattern**: Two Pointers

---

## Problem Summary
Given a string `s`, determine if it is a palindrome after filtering out non-alphanumeric characters and lowercasing everything.

---

## Approach

**Core Idea**
Clean the string first, then use two pointers moving inward from both ends to compare characters.

**Why It Works**
- Removing noise (spaces, punctuation) up front simplifies the comparison loop
- Two pointers converge from both ends — any mismatch immediately returns False
- If they meet or cross without a mismatch, the string is a palindrome

---

## Final Solution (Code)

```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        new_string = ''.join(ch for ch in s if ch.isalnum()).lower()
        left_pointer = 0
        right_pointer = len(new_string) - 1

        while left_pointer < right_pointer:
            if new_string[left_pointer] != new_string[right_pointer]:
                return False

            left_pointer += 1
            right_pointer -= 1

        return True
```

---

## Review Summary

|                   |                                                                  |
| ----------------- | ---------------------------------------------------------------- |
| ✅ What worked    | Clean string preprocessing; correct two-pointer comparison logic |
| ⚠️ Watch out for | Initially: while condition let pointers cross; empty check targeted wrong variable |
| 🧠 Pattern        | [[two-pointers|Two Pointers]]                                                     |
| ⏱️ Complexity    | Time: O(n) / Space: O(n)                                         |

---

## Key Learnings

- `left < right` is the canonical two-pointer exit condition — stops at the midpoint, no wasted iterations
- After cleaning, the empty string case is handled naturally by the while condition — no guard needed
- Checking `if not s` on the original string instead of `new_string` is a subtle bug — always check the variable you're actually using
- The clean-first approach trades O(n) space for simpler loop logic — valid tradeoff for this problem
- Alternative to explore: skip building `new_string` entirely and use two pointers directly on `s`, skipping non-alphanumeric chars in-place (O(1) space)
