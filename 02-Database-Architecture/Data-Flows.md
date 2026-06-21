# Data Flows

[← Back to Database Architecture](Overview.md)

This document describes how data flows between services, emphasizing data ownership and read/write access.

## Data Ownership Matrix

| Entity | Owned By (Write Authority) | Read By | Notes |
|--------|-----------------------------|---------|-------|
| Users | `auth-service` | `gateway` (via JWT), `project-service` | `auth-service` mints JWTs so `gateway` can read identity without hitting the DB. |
| Projects | `project-service` | `task-service`, `analysis-service` | `task-service` queries `project-service` to find the manager for approvals. |
| Tasks | `task-service` | `task-service` | |
| Audit Logs | `analysis-service` | `auth-service` | `auth-service` queries `analysis-service` to show users their own history. |

## Event-Driven Data Flow

When a user modifies data in one service, the state change often needs to propagate. This is handled via the Redis `audit_log` stream.

1. **Write**: User updates a task status to `DONE` via the `task-service`.
2. **Commit**: `task-service` commits the change to the `tasks` table.
3. **Publish**: `task-service` publishes a `status_changed` event to Redis.
4. **Consume (Analytics)**: `analysis-service` reads the event and increments `daily_task_stats` in its database.
5. **Consume (Notifications)**: `notification-worker` reads the event and emails the project manager.
