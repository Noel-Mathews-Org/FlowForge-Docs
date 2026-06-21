# Services

[← Back to Kubernetes Architecture](Overview.md)

## Internal ClusterIP Services
All microservices (excluding the standalone `notification-worker`) are exposed internally via standard Kubernetes `ClusterIP` Services.

The services map from standard internal ports to port `80`.

| Microservice | Target Port | Service Port |
|--------------|-------------|--------------|
| `auth-service` | 8001 | 80 |
| `project-service` | 8002 | 80 |
| `task-service` | 8003 | 80 |
| `analysis-service`| 8004 | 80 |
| `gateway` | 8000 | 80 |
| `frontend` | 3000 | 80 |
