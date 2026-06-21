# ADR-005 Logical Database Separation

## Context
In a microservices architecture, sharing a single database schema creates tight coupling, preventing independent schema evolution and increasing the blast radius of bad queries.

## Decision
FlowForge implements the "Database-per-Service" pattern logically. All services connect to the same PostgreSQL Flexible Server cluster, but each service provisions and interacts with its own distinct logical database.

## Evidence
- `auth-service/models.py`, `project-service/models.py`, etc., demonstrate independent schemas.
- `FlowForge-Terraform/modules/databases/main.tf` provisions databases per environment.

## Benefits
- **Isolation**: A poorly optimized query in the `task-service` is less likely to lock tables needed by the `auth-service`.
- **Evolution**: Teams can run database migrations independently without coordinating downtime.

## Tradeoffs
- **Complex Queries**: Cross-domain queries (e.g., getting a user's name alongside their task) require API joins rather than simple SQL `JOIN` statements, potentially degrading performance.
- **Referential Integrity**: Standard SQL Foreign Keys cannot be used across logical databases.
