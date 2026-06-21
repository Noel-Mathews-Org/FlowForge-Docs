# Disaster Recovery

[← Back to Cloud Architecture](Overview.md)

## RPO & RTO Strategy

Because the architecture relies heavily on GitOps and Infrastructure as Code, the Disaster Recovery strategy favors rebuilding compute environments dynamically rather than backing them up.

1. **Code (Zero Data Loss)**: All application code, Helm manifests, and Terraform infrastructure code are stored securely in GitHub.
2. **Infrastructure (Low RTO)**: In the event of an Azure region failure, the entire infrastructure can be reprovisioned in an alternate region by altering the `location` variable in the Terraform root module and running the pipeline.
3. **Cluster State (Low RTO)**: Once the new AKS cluster comes online, ArgoCD will immediately sync the Helm charts from the Git repository, restoring all services.
4. **Data (Dependent on Azure PaaS)**: 
   - PostgreSQL Flexible server provides automated backups. Geo-redundant backups must be enabled to ensure recovery to a secondary region.
   - Storage accounts utilizing ZRS will survive zonal failures, but Geo-Zone-Redundant Storage (GZRS) is required for cross-region disaster recovery.
