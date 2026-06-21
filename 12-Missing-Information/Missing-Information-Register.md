# Missing Information Register

[← Back to Main Index](../README.md)

During the automated discovery and documentation of the FlowForge platform, the following gaps in available repository evidence were identified.

| Area | Missing Information | Impact |
|------|---------------------|--------|
| Kubernetes Rollouts | The `frontend` Helm chart references `strategy: blueGreen`, but no custom resource definition for an advanced deployment controller (like Argo Rollouts or Flagger) was found in the Helm repository. | Unclear if blue-green deployments actually work, or if this is a configuration error ignored by standard K8s. |
| Pod Disruption Budgets | PDBs are entirely absent from all microservice Helm charts. | Node maintenance (e.g., AKS upgrades) could cause momentary application downtime if all replicas of a service are evicted simultaneously. |
| Cross-Service Consistency | There is no evidence of a Saga pattern or distributed transaction manager across the microservices. | If a project is deleted in `project-service`, orphaned tasks might remain in `task-service` if the event is lost. |
| Database Migrations | No schema migration tool (e.g., Alembic or Flyway) manifests or init-containers were explicitly found. | Unclear how database schemas evolve alongside application code across environments. |
