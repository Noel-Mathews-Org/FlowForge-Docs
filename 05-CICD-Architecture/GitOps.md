# GitOps & ArgoCD

[← Back to CI/CD Architecture](Overview.md)

Instead of utilizing push-based commands like `helm upgrade` within GitHub Actions, FlowForge employs a strict GitOps methodology managed by ArgoCD. Workflows act as updaters to a centralized state repository (`FlowForge-Helm`).

## GitOps Workflow

```mermaid
sequenceDiagram
    participant Dev as Developer
    participant GitApp as FlowForge Repo
    participant Actions as GitHub Actions
    participant ACR as Azure Container Registry
    participant GitHelm as FlowForge-Helm Repo
    participant Argo as ArgoCD

    Dev->>GitApp: Push to Cloud-Track-dev
    Actions->>GitApp: Run Tests & Build Image
    Actions->>ACR: Push Image (SHA: abc1234)
    Actions->>GitHelm: Commit SHA to values-dev.yaml
    Argo->>GitHelm: Detect Git Change
    Argo->>Argo: Reconcile State
    Argo->>AKS: Pull Image & Apply Manifests
```

## ArgoCD Configuration
ArgoCD is configured to synchronize Kubernetes state directly with the `FlowForge-Helm` repository.

- **Dev Application (`argocd-dev-app.yaml`)**:
  - Automatically syncs the `Helm` directory from the `Cloud-Track-dev` branch of the `FlowForge-Helm` repository to the `flowforge-dev` namespace.
  - Combines `values-common.yaml` and `values-dev.yaml`.
- **Prod Application (`argocd-prod-app.yaml`)**:
  - Automatically syncs the `Helm` directory from the `main` branch of the `FlowForge-Helm` repository to the `flowforge-prod` namespace.
  - Combines `values-common.yaml` and `values-prod.yaml`.

Both applications use automated sync policies with `prune: true` and `selfHeal: true`.
