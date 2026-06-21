# Improvement Recommendations

[← Back to Main Index](../README.md)

Based on the architectural review, the following improvements are recommended, prioritized by impact and effort.

## Phase 1: High Impact, Low Effort (Quick Wins)
1. **Implement Pod Disruption Budgets (PDBs)**: Add a simple `pdb.yaml` to the common Helm chart to guarantee at least 1 replica of each service remains available during node draining.
2. **Remove False Blue-Green Configuration**: Fix the `frontend` Helm chart to use a standard `RollingUpdate` strategy, or properly implement Argo Rollouts to realize the intended deployment strategy.
3. **Change Checkov to Hard Fail**: Remove `soft_fail: true` in the `.github/workflows/terraform-gitops.yml` pipeline to enforce IaC security best practices.

## Phase 2: High Impact, Medium Effort
1. **Implement PgBouncer**: Add a connection pooler to the Kubernetes cluster to manage connections to the PostgreSQL Flexible Server, preventing connection exhaustion during HPA scale-up events.
2. **Enforce mTLS**: Install a lightweight service mesh (like Linkerd) to encrypt internal cluster traffic and guarantee service identity, deprecating the shared `INTERNAL_API_KEY`.
3. **Cost Optimization for Dev**: Parameterize the Terraform modules to bypass the Hub VNet and Azure Firewall for non-production environments.

## Phase 3: High Effort, Strategic Architecture
1. **Implement Saga Pattern**: Replace ad-hoc Redis Streams event publishing with a formal transactional outbox pattern to ensure absolute data consistency across logical databases if the message broker fails.
2. **Multi-Region Disaster Recovery**: Architect a secondary region deployment using Azure Front Door for global routing, Geo-Redundant Storage, and Cross-Region Read Replicas for PostgreSQL.
