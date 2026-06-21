# Database Architecture Overview

[← Back to Main Index](../README.md)

FlowForge employs a multi-tenant PostgreSQL architecture combined with Redis for caching and asynchronous messaging. In production, these are implemented via Azure Database for PostgreSQL Flexible Server and Azure Managed Redis (Enterprise).

## Principles

1. **Logical Separation**: Each microservice maintains its own logical database schema to enforce isolation, adhering to the Database-per-Service microservice pattern.
2. **High Availability**: The database layer is deployed in a Zone-Redundant configuration across availability zones (Zones 2 & 3).
3. **No Direct Reads**: Services cannot read each other's databases directly. They must use internal HTTP APIs to request data owned by another service.

## Database Inventory

| Database Component | Service Owner | Primary Role |
|--------------------|---------------|--------------|
| Auth DB | `auth-service` | Identity, Roles, Invites |
| Project DB | `project-service` | Project Metadata, Memberships |
| Task DB | `task-service` | Tasks, Approvals, Comments |
| Analysis DB | `analysis-service` | Audit Logs, AI Tokens, Metrics |
| Rate Limit Cache | `gateway` | API Rate Limiting |
| Stream (`audit_log`) | Redis Broker | Asynchronous Event Hub |

## Related Documents
* [Entity Relationship Diagram](ERD.md)
* [Data Flows](Data-Flows.md)
* [Table Index](Tables/README.md)
