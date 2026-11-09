---
type: concept
status: answered
tags: [dsa]
---

# Frequency Count

Count how many times each distinct value appears, usually with a [[hashmap]] — or, when
the domain is small and fixed (26 lowercase letters, ASCII bytes), a plain fixed-size
array indexed by an offset. The array version is effectively O(1) space since its size
never grows with the input.

**When to recognize it:** the question is about *how many times* something occurs
(anagram checks, "most frequent," character counting) rather than plain membership
(that's [[hashset]]). If the alphabet is bounded (e.g. `'a'...'z'`), prefer
`Array(repeating: 0, count: 26)` over a dictionary — simpler and avoids hashing
overhead.

**Swift skeleton:**
```swift
func isAnagram(_ s: String, _ t: String) -> Bool {
    guard s.count == t.count else { return false }
    var freq = Array(repeating: 0, count: 26)
    let aValue = Character("a").asciiValue!

    for char in s { freq[Int(char.asciiValue! - aValue)] += 1 }
    for char in t { freq[Int(char.asciiValue! - aValue)] -= 1 }

    return freq.allSatisfy { $0 == 0 }
}
```

**Problems:**
- [[Day8]] — 242. Valid Anagram (fixed-size array of 26 as a HashMap substitute)

**Related:** [[hashset]] · [[hashmap]] · [[bucket-sort]]
