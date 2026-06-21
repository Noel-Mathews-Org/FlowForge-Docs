# Reliability & High Availability

[← Back to Cloud Architecture](Overview.md)

FlowForge ensures uptime and resilience through multiple architectural layers.

## Zone Redundancy
- **AKS**: Node pools span availability zones 2 and 3.
- **PostgreSQL**: Flexible Server is deployed as `ZoneRedundant` across zones 2 and 3.
- **Storage**: Azure Blob Storage utilizes Zone-Redundant Storage (ZRS).

## Application Resiliency
- The application layer is strictly stateless. If a zone fails, the Kubernetes scheduler and Horizontal Pod Autoscaler (HPA) will automatically recreate pods on available nodes in surviving zones.
- The GitOps engine (ArgoCD) continuously monitors the state of the cluster and ensures self-healing if cluster resources are accidentally deleted.

## Monitoring & Alerting
Azure Monitor is integrated deeply into the architecture:
- **Log Analytics & App Insights**: Deployed in the Hub VNet.
- **Alerts**:
  - Application Gateway: Triggers if `5xx` failures > 10.
  - PostgreSQL: Triggers if CPU > 80%.
  - Redis: Triggers if Memory > 80%.
  - Key Vault: Triggers if Availability drops below 100%.
