---
type: concept
status: answered
tags: [dsa]
---

# Space Complexity

Count the extra memory an algorithm uses that scales with input size — not the input
itself, and not the required output (by convention, a function's return value usually
doesn't count toward its own space complexity).

**The intuition check:** does the number of extra variables/structures grow if the
input grows? A handful of scalar variables (`var best = 0`) stays O(1) no matter how
big the input is. A fixed-size array (e.g. 26 slots for lowercase letters) is also
O(1) — its size never scales with `n`. A dictionary, set, or array whose size tracks
the input (one entry per character seen, one bucket per element) is O(n).

**Common trip-ups seen across sessions:**
- An output array the problem asks you to return is usually excluded from the space
  count by convention ([[prefix-suffix-products]]).
- A "running variable" can often replace a full auxiliary array — always ask "do I
  need the whole array, or just the latest accumulated value?" before allocating one.
- Any growing structure (a stack, a visited set) is O(n), even if it feels like
  "just bookkeeping" ([[stack]]).

**Related:** [[prefix-suffix-products]] · [[stack]] · [[hashset]]
