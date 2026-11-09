---
type: dsa-session
date: 2026-04-27
tags: [dsa, stack]
problems: [20]
patterns: ["[[stack]]"]
---

# 📘 Daily DSA Session Notes
**Date**: 2026-04-27
**Problem**: 20. Valid Parentheses
**Topic / Pattern**: Stack

---

## Problem Summary
Given a string of bracket characters, determine if every opening bracket is closed by the correct type of closing bracket, in the correct order.

---

## Approach

**Core Idea**
Use a stack and a closing→opening bracket map. For each character: if it's a closing bracket, check the top of the stack for its match — if missing or wrong, return False, else pop. If it's an opening bracket, push it. At the end, the stack must be empty.

**Why It Works**
- The stack enforces ordering — the most recently opened bracket must be closed first (LIFO matches the nesting requirement)
- Mapping closing → opening brackets allows a single dict lookup instead of branching comparisons
- The final `len(stack) == 0` check catches inputs like `"(("` where all chars are openers and nothing fails mid-loop

---

## Final Solution

```python
class Solution:
    def isValid(self, s: str) -> bool:
        parentheses_dict = {
            ')': '(',
            '}': '{',
            ']': '['
        }
        parentheses_stack = []

        for element in s:
            if element in parentheses_dict:
                if not parentheses_stack or parentheses_stack[-1] != parentheses_dict[element]:
                    return False
                else:
                    parentheses_stack.pop()
            else:
                parentheses_stack.append(element)

        return len(parentheses_stack) == 0
```

---

## Review Summary

|                   |                                                                 |
| ----------------- | --------------------------------------------------------------- |
| ✅ What worked    | Clean dict mapping; correct empty-stack guard; solid trace through edge cases |
| ⚠️ Watch out for | Space complexity — any growing auxiliary structure is O(n), not O(1) |
| 🧠 Pattern        | [[stack|Stack]] — bracket matching is the canonical stack problem          |
| ⏱️ Complexity     | Time: O(n) / Space: O(n)                                        |

---

## Key Learnings

- Space complexity counts **all** memory that scales with input — not just the return value
- Mapping closing → opening (not the reverse) is the natural direction for stack-based bracket validation
- The empty-stack check (`not parentheses_stack`) before `[-1]` is essential — missing it causes a runtime error on leading closing brackets
- Final `len(stack) == 0` is required to catch the "all openers, no closers" case
