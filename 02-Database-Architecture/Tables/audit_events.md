# Table: audit_events

[← Back to Tables Index](README.md)

**Owned By**: `analysis-service`

## Purpose
Acts as the system-wide immutable ledger of events, ingested from the Redis `audit_log` stream.

## Schema

| Column Name | Data Type | Constraints | Description |
|-------------|-----------|-------------|-------------|
| `id` | `uuid` | Primary Key | Unique identifier for the event. |
| `event_type` | `varchar` | Not Null | Type of event (e.g., `task_created`). |
| `actor_id` | `uuid` | Nullable | Logical FK to `users.id` who triggered the event. |
| `payload` | `jsonb` | Not Null | Flexible payload containing event specifics. |
| `created_at` | `timestamp`| Default `now()` | When the event was recorded. |

## Data Flow
Written exclusively by the `stream_consumer` background worker within the `analysis-service`. Read by the AI and analytics endpoints.
