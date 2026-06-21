# Reliability Findings

[← Back to Main Index](../README.md)

## Reliability Assessment

| Finding | Risk Level | Details |
|---------|------------|---------|
| Missing PDBs | Medium | The lack of Pod Disruption Budgets in the Helm charts means voluntary disruptions (e.g., AKS node upgrades) could momentarily evict all replicas of a critical service, causing brief outages. |
| Single Point of Failure (Ingress) | Low | While the Application Gateway is highly available within its subnet, the architecture lacks multi-region routing (e.g., Azure Front Door) to survive a total regional failure. |
| Redis Eviction Policy | Medium | The `notification-worker` relies on Redis Streams. If consumer groups fall behind and the stream size isn't capped (XMAXLEN), Redis memory will fill up, potentially causing an OOM crash. |
| Database Connection Pooling | Medium | Services connect directly to PostgreSQL. Without a connection pooler like PgBouncer, a rapid scale-up event could exhaust the database connection limits. |
