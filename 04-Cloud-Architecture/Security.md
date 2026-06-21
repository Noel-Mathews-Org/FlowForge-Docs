# Security & Encryption

[← Back to Cloud Architecture](Overview.md)

## Azure Key Vault
- **Purpose**: Centralized storage for sensitive application strings, connection secrets, and Entra ID client secrets.
- **Security**: Public network access is disabled. Accessible only via Private Endpoint (`privatelink.vaultcore.azure.net`).
- **Access**: The `Key Vault Secrets User` RBAC role is assigned to the `mi-flowforge-app-<env>` Managed Identity, allowing Kubernetes pods to read secrets seamlessly.
- **Protection**: Configured with Purge Protection to prevent accidental or malicious deletion of secrets.

## WAF & Application Gateway
- The Application Gateway utilizes the `WAF_v2` SKU.
- It is attached to a Web Application Firewall policy enforcing **OWASP 3.2 rules** in `Prevention` mode.

## Network Isolation
- All PaaS resources (Redis, Key Vault, Storage) have `public_network_access_enabled = false` and rely solely on Private Endpoints within `snet-pe`.
- PostgreSQL uses native VNet integration (Subnet Delegation into `snet-db`).
