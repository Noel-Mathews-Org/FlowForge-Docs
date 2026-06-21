# Identity & Access Management

[← Back to Cloud Architecture](Overview.md)

FlowForge utilizes Azure Entra ID (formerly Azure AD) for comprehensive identity management across both users and infrastructure.

## Managed Identities
The Terraform deployment relies heavily on User-Assigned Managed Identities instead of Service Principals:
- `mi-flowforge-app-<env>`: The primary identity assumed by the application workloads inside AKS.
- `mi-<cluster-name>-cp`: The identity used by the AKS control plane to manage underlying Azure resources (like the Load Balancer).
- `mi-github-actions-prod`: A dedicated identity used by GitHub Actions workflows.

## Workload Identity Federation
FlowForge does not use secrets for Kubernetes to access Azure resources. It uses OIDC (OpenID Connect) federation:
- **AKS to Azure**: The AKS OIDC Issuer maps Kubernetes `ServiceAccounts` (e.g., `system:serviceaccount:flowforge-<env>:flowforge-<env>-<svc>`) to the `mi-flowforge-app-<env>` identity.
- **GitHub to Azure**: GitHub Actions workflows authenticate to Azure using OIDC tokens (`https://token.actions.githubusercontent.com`). The trust is established via subjects matching specific repositories and branches (`repo:Noel-Mathews-Org/FlowForge:ref:refs/heads/<branch>`).

## RBAC Assignments
Key infrastructure permissions are granted natively via Azure RBAC:
- **ACR Pull**: The AKS Node identity (`kubelet_identity`) has `AcrPull` permission over the Azure Container Registry.
- **Ingress Controller**: The Application Gateway Ingress Controller (AGIC) identity has `Network Contributor` on the Spoke VNet and `Contributor` on the AppGW.
- **Human Access**: 
  - `devops_group_object_id` is granted `Azure Kubernetes Service RBAC Cluster Admin`.
  - `devtest_group_object_id` is granted `Azure Kubernetes Service RBAC Reader`, specifically scoped to the `/namespaces/flowforge` namespace.
