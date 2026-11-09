---
type: concept
status: answered
tags: [database, system-design, bridge]
---

# Message queue

A buffer (RabbitMQ, Kafka, SQS) between app and DB. The app drops a message and responds immediately; a worker writes to the DB at a controlled pace. Absorbs traffic spikes. Tradeoff: writes aren't instant (eventual consistency).

## Under the hood (DSA)
- Idempotent consumers deduplicate already-processed message IDs with a [[hashset]].
- A priority-ordered worker queue (process the most urgent job first) is a [[heap-priority-queue]].

**Answers:** [[what-is-a-message-queue]]
**Sources:** [[bytebytego]]
**Related:** [[cache-redis]]
