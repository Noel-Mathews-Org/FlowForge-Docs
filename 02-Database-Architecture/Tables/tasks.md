# Table: tasks

[← Back to Tables Index](README.md)

**Owned By**: `task-service`

## Purpose
Stores kanban tasks and their state within a project.

## Schema

| Column Name | Data Type | Constraints | Description |
|-------------|-----------|-------------|-------------|
| `id` | `uuid` | Primary Key | Unique identifier for the task. |
| `project_id` | `uuid` | Not Null | Logical FK to `projects.id`. |
| `title` | `varchar` | Not Null | Task title. |
| `description` | `text` | Nullable | Task details. |
| `status` | `varchar` | Not Null | Enum: `TODO`, `IN_PROGRESS`, `DONE`, `BLOCKED`. |
| `assignee_id` | `uuid` | Nullable | Logical FK to `users.id`. |
| `needs_approval`| `boolean`| Default `false`| If true, transitions to `DONE` require manager approval. |

## Relationships
- Logically belongs to a `project`.
- Has many `task_comments`.
- Has many `approval_requests`.
