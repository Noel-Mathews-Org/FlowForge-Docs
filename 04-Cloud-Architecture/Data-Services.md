# Data Services

[← Back to Cloud Architecture](Overview.md)

## Azure Database for PostgreSQL Flexible Server
- **Deployment**: Deployed into a dedicated delegated subnet (`snet-db`).
- **High Availability**: Configured as `ZoneRedundant`, spanning Zones 2 and 3.
- **Logical Segregation**: Provisions multiple logical databases dynamically (e.g., `flowforge-dev`, `flowforge-prod`) upon deployment.

## Azure Managed Redis (Enterprise)
- **Deployment**: Utilizes the `EnterpriseCluster` policy.
- **Network Isolation**: Connected exclusively via Azure Private Endpoint (`privatelink.redis.azure.net`) into `snet-pe`.

## Azure Blob Storage
- **Deployment**: Storage accounts are deployed with Zone-Redundant Storage (ZRS).
- **Access Control**: The `Storage Blob Data Contributor` RBAC role is assigned to the `mi-flowforge-app-<env>` managed identity.
- **Containers**: Includes `app-data` for generalized unstructured data and `ai-incident-reports` for AI analysis outputs.
