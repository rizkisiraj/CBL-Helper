---
type: hub
tags: [bridge]
---

# Graph Views

Saved ways to look at the Obsidian Graph View for this vault, mirrored from the color
groups already configured in `.obsidian/graph.json`. Open Graph View, paste the filter
into the search box, and compare against the groups/colors below.

## 1. Database
- Filter: `tag:#database`
- Color: `#3B82F6` (as configured in `graph.json`)
- What to look for: the storage/indexing/query-tuning cluster — `database-index`,
  `b-tree-index`, `explain-vs-explain-analyze`, and friends.

## 2. System Design
- Filter: `tag:#system-design`
- Color: `#F59E0B`
- What to look for: the "scaling a single box" cluster — caching, queues, connection
  pooling, monitoring, latency/throughput measurement.

## 3. Distributed Systems
- Filter: `tag:#distributed-systems`
- Color: `#8B5CF6`
- What to look for: the "scaling out" cluster — Cassandra's architecture, gossip,
  partitioning, replication.

## 4. DSA
- Filter: `tag:#dsa`
- Color: `#22C55E`
- What to look for: algorithm/data-structure patterns from daily practice sessions.
  Before this handoff, this cluster had no edges to any other color — see section 6.

## 5. iOS
- Filter: `tag:#ios`
- Color: `#EC4899`
- What to look for: the iOS/Swift handbook cluster, separate from the backend-leaning
  topics above.

## 🌉 6. Bridges
- Filter: `tag:#bridge`
- Groups (in this order):
  - `tag:#dsa` → `#3E63DD` (blue)
  - `tag:#distributed-systems` → `#8E4EC6` (purple)
  - `tag:#database -tag:#system-design` → `#F76B15` (orange)
  - `tag:#system-design` → `#30A46C` (green)
- What to look for: blue nodes (DSA) linked to a green, orange, or purple node are
  bridges — proof an algorithm/data-structure shows up in a real system. A blue node
  with no non-blue neighbor still needs a bridge; see
  [[dsa-x-system-design|the DSA × System Design hub]] for the current full list and
  what's left to connect.

---
**Related:** [[home]] · [[dsa-x-system-design]] · [[dsa]] · [[system-design]] · [[distributed-systems]]
