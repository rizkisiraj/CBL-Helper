---
type: concept
status: open
tags: [system-design, bridge]
---

# Rate Limiting

Cap how many requests a client can make in a given time period, rejecting (or
queueing) the rest. Protects a service from being overwhelmed by one noisy caller.
The sliding-window counter variant: keep a count of requests in the last N seconds and
reject once it's exceeded, rather than resetting the count at fixed clock boundaries
(which lets a client burst right at the boundary — the "fixed window" flaw).

## Under the hood (DSA)
- A sliding-window rate limiter is literally the [[sliding-window]] pattern: track the count of requests inside a moving `[now - N seconds, now]` window instead of a fixed bucket.
- Per-client counters are stored in a [[hashmap]] keyed by client ID.

**Related:** [[sliding-window]] · [[hashmap]] · [[tps]]
