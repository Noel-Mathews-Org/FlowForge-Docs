# Architecture Discovery Log

[← Back to Main Index](../README.md)

## Discovered Components
- **Microservices**: Frontend, Gateway, Auth, Project, Task, Analysis, Notification Worker.
- **Databases**: PostgreSQL Flexible Server (logical DBs per service).
- **Caching/Messaging**: Redis Enterprise (Rate limiting, Redis Streams).
- **Infrastructure**: Azure Virtual Networks (Hub-Spoke), Application Gateway WAF, Azure Firewall, Azure Key Vault, Azure Blob Storage.
- **Compute**: Azure Kubernetes Service (AKS) Private Cluster.
- **CI/CD**: GitHub Actions, ArgoCD.

## Undiscovered Components & Anomalies
- **Deployment Strategy Anomaly**: The `frontend` Helm chart specifies a `strategy: blueGreen` inside a standard `Deployment` kind. Native Kubernetes does not support this. It implies the existence of a custom controller (like Argo Rollouts), but no such controller manifests were found in the Helm repository.
- **StatefulSets**: No StatefulSets were found in the Helm repository.
- **PDBs**: No Pod Disruption Budgets were found.
- **Kubernetes Secrets**: No native K8s Secrets are used; all secrets are managed via Azure Key Vault.
