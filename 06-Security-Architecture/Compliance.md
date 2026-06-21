# Compliance

[← Back to Security Architecture](Overview.md)

FlowForge's architecture supports standard enterprise compliance requirements (e.g., SOC2, ISO 27001):

- **Encryption at Rest**: Azure PaaS services (PostgreSQL, Storage, Key Vault) encrypt data at rest by default using Microsoft-managed keys.
- **Encryption in Transit**: All external traffic is TLS-encrypted via Application Gateway. Internal traffic relies on network isolation.
- **Audit Trails**: The `analysis-service` maintains an immutable ledger of system events in the `audit_events` table.
- **Access Control**: Entra ID integration ensures centralized offboarding and access reviews.
