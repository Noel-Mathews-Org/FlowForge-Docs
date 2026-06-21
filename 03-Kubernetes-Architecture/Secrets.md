# Secret Management

[← Back to Kubernetes Architecture](Overview.md)

## Secretless Architecture

FlowForge intentionally avoids using native Kubernetes `Secret` resources for sensitive application configurations.

Instead, the platform relies on **Azure Key Vault** integrated with **Azure Entra Workload Identity**.

## Workload Identity Implementation

1. **OIDC Issuer**: The AKS cluster has an OIDC issuer enabled.
2. **Service Accounts**: Each microservice provisions a Kubernetes `ServiceAccount` (e.g., in `Helm/charts/auth-service/templates/serviceaccount.yaml`).
3. **Annotations**: The ServiceAccount is annotated to bind to an Azure Managed Identity:
   ```yaml
   azure.workload.identity/client-id: {{ .Values.global.azure.managedIdentityClientId }}
   ```
4. **Pod Labels**: Pods are labelled to instruct the Workload Identity webhook to inject the necessary environment variables:
   ```yaml
   azure.workload.identity/use: "true"
   ```
5. **Runtime**: The application starts, detects the injected OIDC token, and authenticates directly to Azure Key Vault to pull secrets like `DATABASE_URL` and `JWT_SECRET` into memory.

## Benefits
- Secrets are never visible in etcd, GitHub, or ArgoCD.
- No requirement for tools like ExternalSecrets or SealedSecrets to sync data into the cluster.
