# ADR-004 Event-Driven Redis

## Context
Certain operations, like sending emails or aggregating audit logs, are too slow to be executed synchronously during a standard API request cycle.

## Decision
Redis Streams was selected as the asynchronous message broker. Services publish events to the `audit_log` stream, and background workers consume from it.

## Evidence
- `docker-compose.yml` includes Redis.
- `notification-worker/worker.py` consumes the stream.
- `analysis-service` contains `workers/stream_consumer.py`.

## Benefits
- **Decoupling**: The `task-service` does not need to know the `SMTP_HOST` to send an email; it just emits an event.
- **Performance**: User requests return immediately without waiting for third-party SMTP or AI analytics processing.

## Tradeoffs
- **At-Least-Once Delivery**: Redis Streams guarantee at-least-once delivery, meaning consumers must be idempotent to handle occasional duplicate messages gracefully.

## Risks
- If Redis memory fills up and the stream is not capped or trimmed properly, the Redis instance could crash.
