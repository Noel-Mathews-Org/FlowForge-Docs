# Security Dependency Matrix

[← Back to Main Index](../README.md)

| Component | Identity Source | Authorization Source | Secret Store |
|-----------|-----------------|----------------------|--------------|
| Users (Frontend) | Entra ID / Local DB | Application Roles (in DB/JWT) | N/A |
| Gateway | JWT (Signature Check)| None (Passes claims) | Azure Key Vault (`JWT_SECRET`) |
| Microservices | `X-User-*` Headers | Dependency Injection (`require_role`) | Azure Key Vault |
| AKS Pods | Workload Identity | Azure RBAC | Azure Key Vault |
| GitHub Actions | Workload Identity (OIDC)| Entra ID Federated Credentials | GitHub Secrets |
