# Troubleshooting

[← Back to Main Index](../README.md)

## Common Scenarios

### 1. Application Gateway Returning 502 Bad Gateway
- **Cause**: The AGIC controller cannot reach the AKS nodes, or the pods are crashing.
- **Action**: Check NSGs on `snet-aks` and ensure the `gateway` pod is running (`kubectl get pods -n flowforge-prod`).

### 2. Database Connection Timeouts
- **Cause**: PostgreSQL is isolated in `snet-db` and might be unreachable.
- **Action**: Verify the AKS subnet is allowed in `nsg-db`. Connect to the Hub VPN Gateway to query the database directly using `psql` to test connectivity.

### 3. ArgoCD Sync Failing
- **Cause**: A misconfiguration in `values-prod.yaml` or a chart dependency error.
- **Action**: Login to the ArgoCD dashboard. Check the Sync Status and the error message provided by the Helm rendering engine.
