# Table: users

[← Back to Tables Index](README.md)

**Owned By**: `auth-service`

## Purpose
Stores the core identity for all users in the FlowForge platform, linking local authentication with Azure Entra ID.

## Schema

| Column Name | Data Type | Constraints | Description |
|-------------|-----------|-------------|-------------|
| `id` | `uuid` | Primary Key | Unique identifier for the user. |
| `email` | `varchar` | Unique, Not Null | User's email address. |
| `password_hash` | `varchar` | Nullable | Hashed password for local auth (null for SSO-only). |
| `entra_oid` | `varchar` | Unique, Nullable | The Azure Entra ID Object ID for SSO mapping. |
| `role` | `varchar` | Not Null | User's RBAC role (e.g., `member`, `manager`). |
| `is_active` | `boolean` | Default `true` | Whether the user account is enabled. |

## Relationships
- Has many `invitations`
- Has many `refresh_tokens`
- Has many `notifications`
