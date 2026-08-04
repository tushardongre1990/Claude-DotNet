# Messaging & Event-Driven Architecture

Status: **Not started**

## Planned coverage
- Message queues vs pub/sub — diagram comparing point-to-point and fan-out
- RabbitMQ basics from .NET (exchanges, queues, bindings, routing keys)
- Kafka basics from .NET (topics, partitions, consumer groups, offsets) — how it differs from a traditional queue
- Azure Service Bus (queues, topics/subscriptions) as the managed alternative
- At-least-once vs exactly-once vs at-most-once delivery semantics
- Idempotent consumers (handling duplicate delivery) — ties into [[18-API-Design-Best-Practices]] idempotency keys
- Outbox pattern (guaranteeing message delivery alongside a DB transaction) — sequence diagram
- Event-driven vs event sourcing (brief distinction; full event sourcing is often out of scope but good to recognize)
- Dead-letter queues, poison messages, retry policies
