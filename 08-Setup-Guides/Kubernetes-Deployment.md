# Kubernetes Deployment

[← Back to Main Index](../README.md)

Because FlowForge utilizes GitOps, manual Kubernetes deployment using `kubectl` or `helm upgrade` is discouraged.

## Helm Configuration

1. **Namespaces**: These are created automatically by ArgoCD. Do not create them manually.
2. **Secrets**: Do not create Kubernetes Secrets. Ensure Azure Key Vault is populated with `JWT_SECRET`, `DATABASE_URL`, etc. The Workload Identity will pull these at runtime.
3. **ConfigMaps**: Update `values-dev.yaml` or `values-prod.yaml` in the `FlowForge-Helm` repository. This will automatically update the `flowforge-config` ConfigMap in the cluster.
4. **Ingress**: The ingress routing is handled by the umbrella chart. Ensure your public DNS is pointing to the IP address of the Application Gateway provisioned by Terraform.
