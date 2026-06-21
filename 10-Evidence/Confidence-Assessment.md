# Confidence Assessment

[← Back to Main Index](../README.md)

This table outlines the confidence level in the generated documentation based on the depth of evidence found during the repository audit.

| Area | Confidence | Reason |
|------|------------|--------|
| Application Microservices | High | Clear FastAPI routers and Next.js configs present in the `FlowForge` repo. |
| Database Relationships | Medium | While SQLAlchemy models are clear, inter-service relationships (e.g., Task -> Project) are logical rather than enforced by hard foreign keys, making exact constraints slightly inferred. |
| Kubernetes Architecture | High | Helm charts and values files explicitly declare the cluster state. |
| Cloud Infrastructure | High | Terraform modules explicitly define all Azure resources, networks, and identities. |
| Security Posture | High | NSGs, Firewall rules, WAF policies, and Workload Identity federations are fully codified in Terraform. |
| CI/CD Pipelines | High | GitHub Actions YAML files clearly define the build, scan, and deploy steps. |
| ArgoCD Rollout Mechanism | Low | The `frontend` deployment mentions `strategy: blueGreen`, but no Argo Rollouts configuration was found. It is unclear if this strategy actually works in the cluster. |
