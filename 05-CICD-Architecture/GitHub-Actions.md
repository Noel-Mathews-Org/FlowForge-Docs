# GitHub Actions Workflows

[← Back to CI/CD Architecture](Overview.md)

## Reusable Workflows

A central `.github` repository stores a reusable CI pipeline (`_ci-reusable.yml`). This acts as the standard pipeline for all microservices.

**Key Steps:** 
1. SonarQube code quality scan.
2. Snyk vulnerability test & monitor for Node/Python dependencies.
3. Builds Docker image (fetching Entra Client/Tenant IDs from `values-dev.yaml`).
4. Trivy container vulnerability scan.
5. Pushes Docker image to Azure Container Registry (ACR) with the short commit SHA.
6. Implements GitOps by updating the image tag in `Helm/values-dev.yaml` within the `FlowForge-Helm` repository on the `Cloud-Track-dev` branch.
7. Sends an email notification with pipeline results.

## Service CI Workflows

Each microservice in the `FlowForge` repository has a `ci-*.yml` file (e.g., `ci-auth-service.yml`). These workflows trigger on pushes to the `Cloud-Track-dev` branch for changes in their specific service directories, and call the reusable workflow `_ci-reusable.yml` via `uses: Noel-Mathews-Org/.github/.github/workflows/_ci-reusable.yml@main`.

## Environments & Approvals

- **GitHub Environments**: Pipelines explicitly use the GitHub `environment` context.
- **Approval Gates**: Deployments or destructive actions targeting the `prod` environment (like `terraform-destroy.yml`) pause for manual approval configured within GitHub repository settings.
