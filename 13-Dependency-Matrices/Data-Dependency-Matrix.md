# Data Dependency Matrix

[← Back to Main Index](../README.md)

| Data Entity | Primary Owner | Read By | Written By |
|-------------|---------------|---------|------------|
| Users | `auth-service` | `gateway`, `project-service` | `auth-service` |
| Projects | `project-service` | `task-service` | `project-service` |
| Tasks | `task-service` | `task-service` | `task-service` |
| Audit Events | `analysis-service`| `auth-service` | `analysis-service` (Via Stream) |
| Rate Limits | Redis Cache | `gateway` | `gateway` |
