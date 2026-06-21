# Namespaces & GitOps

[← Back to Kubernetes Architecture](Overview.md)

## Environment Separation

The cluster isolates environments logically using Kubernetes Namespaces.

| Environment | Branch | ArgoCD App | Target Namespace |
|-------------|--------|------------|------------------|
| Development | `Cloud-Track-dev` | `argocd-dev-app` | `flowforge-dev` |
| Production | `main` | `argocd-prod-app` | `flowforge-prod` |

## ArgoCD Synchronization

ArgoCD is responsible for namespace lifecycle management:
- Namespaces are automatically provisioned by ArgoCD via the `CreateNamespace=true` sync option.
- Sync policies are set to automatic with `prune: true` and `selfHeal: true`, ensuring any manual `kubectl` tampering is immediately reverted to the Git state.
