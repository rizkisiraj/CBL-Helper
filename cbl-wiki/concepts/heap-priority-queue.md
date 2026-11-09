---
type: concept
status: open
tags: [dsa, bridge]
---

# Heap / Priority Queue

A tree-shaped structure that keeps the smallest (min-heap) or largest (max-heap)
element accessible in O(1), with O(log n) insert and remove-top. Unlike a sorted array,
it doesn't keep *everything* ordered — just enough structure to always answer "what's
the extreme value right now."

**When to recognize it:** "top K," "Kth largest/smallest," "merge K sorted things," or
any streaming problem that needs the current min/max without re-sorting from scratch
every time a new element arrives.

**Swift skeleton** (binary heap behavior, via a sorted insert for illustration —
Swift's standard library has no built-in heap, so real code reaches for a small custom
struct or `Heap` from swift-collections):
```swift
struct MinHeap {
    private var data: [Int] = []

    mutating func insert(_ value: Int) {
        data.append(value)
        var i = data.count - 1
        while i > 0, data[(i - 1) / 2] > data[i] {
            data.swapAt(i, (i - 1) / 2)
            i = (i - 1) / 2
        }
    }

    func peekMin() -> Int? { data.first }
}
```

## In real systems
- A [[message-queue]] worker pool that always processes the most urgent job first is a priority queue ordered by priority instead of arrival time.
- Streaming the running median/percentile of incoming [[latency-percentiles|latencies]] without re-sorting every request is the classic two-heap (max-heap + min-heap) pattern.

**Related:** [[bucket-sort]] · [[frequency-count]]
