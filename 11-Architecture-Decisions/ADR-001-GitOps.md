# ADR-001 GitOps Deployment

## Context
The platform requires consistent, secure, and auditable deployments to Kubernetes across multiple environments (dev and prod) without granting CI/CD pipelines direct administrative access to the cluster.

## Decision
ArgoCD was selected as the deployment engine, implementing a strict GitOps model.

## Evidence
- `.github/workflows/_ci-reusable.yml` commits image SHAs to `FlowForge-Helm`.
- `FlowForge-Helm/argocd/argocd-dev-app.yaml` defines the sync policy.

## Benefits
- **Security**: CI pipelines do not need Kubernetes credentials.
- **Auditability**: Every change to the cluster state is recorded as a Git commit in the Helm repository.
- **Self-Healing**: Manual tampering in the cluster is automatically reverted by ArgoCD.

## Tradeoffs
- Additional operational complexity of managing the ArgoCD control plane.
- Separation of CI (building images) and CD (updating manifests) across multiple repositories can be confusing for new developers.

## Risks
- If the ArgoCD controller fails, deployments are halted until it is restored.
