---
type: concept
status: answered
tags: [cbl]
---

# Message queue

A buffer (RabbitMQ, Kafka, SQS) between app and DB. The app drops a message and responds immediately; a worker writes to the DB at a controlled pace. Absorbs traffic spikes. Tradeoff: writes aren't instant (eventual consistency).

**Answers:** [[what-is-a-message-queue]]
**Sources:** [[bytebytego]]
**Related:** [[cache-redis]]
