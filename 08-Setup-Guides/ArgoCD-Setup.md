# ArgoCD Setup

[← Back to Main Index](../README.md)

## Installation
If ArgoCD is not provisioned by Terraform natively:
```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

## Projects & Applications
1. Apply the Dev GitOps manifest:
   ```bash
   kubectl apply -f argocd/argocd-dev-app.yaml
   ```
2. Apply the Prod GitOps manifest:
   ```bash
   kubectl apply -f argocd/argocd-prod-app.yaml
   ```

## Sync Policies
Both applications are configured with:
```yaml
syncPolicy:
  automated:
    prune: true
    selfHeal: true
  syncOptions:
    - CreateNamespace=true
```

## RBAC
ArgoCD should be integrated with Azure Entra ID for Single Sign-On (SSO) using an OIDC connector in the `argocd-cm` ConfigMap to restrict access to Platform Administrators.
