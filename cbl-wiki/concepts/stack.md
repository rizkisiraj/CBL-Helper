---
type: concept
status: answered
tags: [dsa]
---

# Stack

A LIFO structure — the last thing pushed is the first thing popped. It's the natural
fit whenever "most recently opened/seen" needs to be matched or undone before anything
earlier can be resolved.

**When to recognize it:** bracket/parenthesis matching, nested structure validation, or
any "undo the most recent thing first" requirement. For bracket matching specifically:
map closing brackets → their expected opening bracket (not the reverse), push openers,
and on a closer check the top of the stack for a match. The stack must end **empty** —
that final check catches inputs that are all openers with nothing to close them.

**Swift skeleton:**
```swift
func isValid(_ s: String) -> Bool {
    let pairs: [Character: Character] = [")": "(", "]": "[", "}": "{"]
    var stack: [Character] = []

    for char in s {
        if let opener = pairs[char] {
            guard stack.popLast() == opener else { return false }
        } else {
            stack.append(char)
        }
    }
    return stack.isEmpty
}
```

**Problems:**
- [[Day11]] — 20. Valid Parentheses

**Related:** [[two-pointers]]
