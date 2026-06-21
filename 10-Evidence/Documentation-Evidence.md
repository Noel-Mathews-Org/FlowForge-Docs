# Documentation Evidence

[← Back to Main Index](../README.md)

Every architectural statement in this documentation is traceable back to repository evidence. This table maps the documentation sections to the raw source files analyzed during the discovery phase.

| Documentation Section | Repository | File(s) Used | Evidence |
|-----------------------|------------|--------------|----------|
| Application Overview | `FlowForge` | `docker-compose.yml`, `frontend/package.json`, `gateway/main.py` | Service configurations and runtimes |
| Database Architecture | `FlowForge` | `*-service/models.py` | SQLAlchemy ORM models define schemas |
| Kubernetes Namespaces | `FlowForge-Helm` | `argocd/argocd-dev-app.yaml`, `argocd/argocd-prod-app.yaml` | Sync options defining `CreateNamespace=true` |
| Ingress Architecture | `FlowForge-Helm` | `Helm/templates/ingress.yaml` | `kubernetes.io/ingress.class: azure/application-gateway` |
| Cloud Networking | `FlowForge-Terraform`| `modules/hub_network/main.tf`, `modules/spoke_network/main.tf` | VNet and Subnet resources |
| Private Endpoints | `FlowForge-Terraform`| `modules/databases/main.tf`, `modules/key_vault/main.tf` | `azurerm_private_endpoint` resources |
| CI/CD Pipelines | `.github` | `.github/workflows/_ci-reusable.yml` | Container build and GitOps push steps |
| Security Scanning | `.github` | `.github/workflows/_ci-reusable.yml` | SonarQube, Snyk, and Trivy action steps |
| Infrastructure Scanning | `FlowForge-Terraform`| `.github/workflows/terraform-gitops.yml` | Checkov action steps |
| Identity & RBAC | `FlowForge-Terraform`| `env/dev/main.tf` | Workload Identity federation block and RBAC assignments |
