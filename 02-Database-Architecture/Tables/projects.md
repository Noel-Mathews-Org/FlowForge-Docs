# Table: projects

[← Back to Tables Index](README.md)

**Owned By**: `project-service`

## Purpose
Stores organizational boundaries for tasks and teams.

## Schema

| Column Name | Data Type | Constraints | Description |
|-------------|-----------|-------------|-------------|
| `id` | `uuid` | Primary Key | Unique identifier for the project. |
| `name` | `varchar` | Not Null | Display name of the project. |
| `manager_id` | `uuid` | Not Null | UUID of the user acting as manager (logical FK to `users.id`). |
| `is_archived`| `boolean` | Default `false` | Soft-delete / archive flag. |

## Relationships
- Has many `project_members` (Join table linking `project_id` and `user_id`).
- Logically contains many `tasks` (enforced via `project_id` in the `task-service`).
