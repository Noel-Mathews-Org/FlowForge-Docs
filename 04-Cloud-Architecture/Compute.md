# Compute Layer (AKS)

[← Back to Cloud Architecture](Overview.md)

## Azure Kubernetes Service (AKS)
FlowForge runs its compute workloads on a private AKS cluster (`private_cluster_enabled = true`).

## Node Configuration
- Uses Virtual Machine Scale Sets (VMSS).
- Nodes are deployed across Availability Zones 2 and 3 for redundancy.

## Cluster Configuration
- **Network**: Uses `azure` CNI. This ensures pods receive real IPs from the Virtual Network.
- **Policies**: Utilizes `calico` network policies.
- **Authentication**: Local accounts are disabled. Relying entirely on Entra ID RBAC.
- **Ingress**: Configured with the Application Gateway Ingress Controller (`ingress_application_gateway`).
