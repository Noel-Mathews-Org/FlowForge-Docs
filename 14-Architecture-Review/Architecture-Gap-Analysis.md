# Architecture Gap Analysis

[← Back to Main Index](../README.md)

## Missing Components & Design Gaps

| Area | Gap Identified | Risk |
|------|----------------|------|
| **Deployment** | Absence of Pod Disruption Budgets (PDBs). | High risk of temporary downtime during scheduled cluster maintenance or node upgrades. |
| **Deployment** | Unbacked `blueGreen` strategy in the `frontend` Helm chart. | False sense of security regarding deployment risk; fallbacks to `RollingUpdate` might drop live user traffic if probes aren't perfectly tuned. |
| **Data Consistency**| Lack of a transactional outbox or saga pattern for microservice data consistency. | Moderate risk of orphaned data. If Redis drops a stream message, the analytical dashboard and downstream services become permanently out of sync with the primary database. |
| **Storage** | No Geo-Redundant backup strategy for PostgreSQL. | Total loss of data if the primary Azure Region suffers an unrecoverable failure. |
