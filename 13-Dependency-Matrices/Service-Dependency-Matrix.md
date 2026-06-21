# Service Dependency Matrix

[← Back to Main Index](../README.md)

| Service | HTTP Calls Out To | Publishes To | Consumes From | External Systems |
|---------|-------------------|--------------|---------------|------------------|
| `gateway` | All Backend Services | - | - | Entra ID (Implicit via Token) |
| `frontend` | `gateway` | - | - | Entra ID (Browser Redirect) |
| `auth-service` | `project-service`, `analysis-service` | - | - | Entra ID (Graph API), SMTP |
| `project-service` | `auth-service` | `audit_log` Stream | - | - |
| `task-service` | `project-service`, `auth-service` | `audit_log` Stream | - | - |
| `analysis-service`| Azure AI Foundry | - | `audit_log` Stream | Azure AI Foundry / OpenAI |
| `notification-worker`| - | - | `audit_log` Stream | SMTP |
