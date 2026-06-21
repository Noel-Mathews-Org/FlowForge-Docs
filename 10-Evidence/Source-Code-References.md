# Source Code References

[← Back to Main Index](../README.md)

## FlowForge (Application Repository)
- `docker-compose.yml`: Defines local development architecture and Redis usage.
- `gateway/main.py`, `gateway/proxy.py`, `gateway/auth.py`: Defines API Gateway routing, rate-limiting, and JWT verification.
- `auth-service/models.py`, `project-service/models.py`, `task-service/models.py`, `analysis-service/models.py`: Defines PostgreSQL logical databases and schemas.
- `notification-worker/worker.py`: Defines the Redis Stream consumer for email notifications.
- `frontend/package.json`: Defines the Next.js frontend tech stack.

## FlowForge-Helm (Kubernetes Repository)
- `Helm/Chart.yaml`: Defines the umbrella chart and microservice sub-charts.
- `Helm/values-common.yaml`, `Helm/values-dev.yaml`, `Helm/values-prod.yaml`: Defines environment configurations and Entra ID secrets mapping.
- `Helm/templates/ingress.yaml`: Defines Application Gateway ingress rules.
- `Helm/templates/global-config.yaml`: Defines the `flowforge-config` ConfigMap.
- `argocd/argocd-dev-app.yaml`, `argocd/argocd-prod-app.yaml`: Defines ArgoCD GitOps sync parameters.

## FlowForge-Terraform (Infrastructure Repository)
- `env/dev/main.tf`, `env/prod/main.tf`: Root modules defining module orchestration and identity federation loops.
- `env/dev/providers.tf`: Defines remote state configuration using OIDC.
- `modules/spoke_network/main.tf`: Defines NSGs, subnets, and subnet delegation.
- `modules/firewall/main.tf`: Defines Egress routing and UDRs.
- `modules/aks/main.tf`: Defines the private AKS cluster and VMSS configurations.
- `modules/databases/main.tf`: Defines PostgreSQL Flexible Server and Redis Enterprise.

## .github (CI/CD Repository)
- `.github/workflows/_ci-reusable.yml`: Defines the primary CI flow with SonarQube, Snyk, and Trivy.
- `FlowForge/.github/workflows/prod-release.yml`: Defines CD production tag promotion.
- `FlowForge-Terraform/.github/workflows/terraform-gitops.yml`: Defines Terraform deployment and Checkov scanning.
