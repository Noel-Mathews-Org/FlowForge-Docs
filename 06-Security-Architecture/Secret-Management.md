# Secret Management

[← Back to Security Architecture](Overview.md)

FlowForge utilizes a secretless architecture where possible.

## Workload Identity
AKS Pods assume User-Assigned Managed Identities (`mi-flowforge-app-<env>`) via OIDC federation. This allows pods to authenticate to Azure resources (like Key Vault and Blob Storage) without connection strings.

## Azure Key Vault
- Serves as the central repository for database passwords, API keys, and Entra ID client secrets.
- Accessed via Workload Identity.
- Configured with Purge Protection to prevent accidental deletion.
- Inaccessible from the public internet (secured via Private Endpoint).

## GitHub Secrets
Secrets required for GitHub Actions (e.g., Terraform State Storage keys) are stored in GitHub Repository/Environment Secrets. OIDC federation is used by GitHub Actions to assume the `mi-github-actions-prod` identity to push images to ACR.
