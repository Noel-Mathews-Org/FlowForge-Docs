# Application Pipelines

[← Back to CI/CD Architecture](Overview.md)

## CI (Continuous Integration) Flow
When a developer pushes to `Cloud-Track-dev`, GitHub Actions:
1. Builds the container.
2. Tags it with a short SHA.
3. Scans it with Trivy.
4. Pushes to Azure Container Registry (ACR).
5. Commits the SHA to `values-dev.yaml` in the Helm repository.

## CD (Continuous Deployment / Release) Flow
Production releases are driven by GitHub Releases via `prod-release.yml`.

1. **Trigger**: Developer creates a GitHub Release (e.g., `frontend-v1.2.0`).
2. **Tag Discovery**: Actions parses the release tag and pulls the existing development image SHA from the `FlowForge-Helm` dev manifest (`values-dev.yaml`).
3. **Promotion**: Retags the image with the release version, scans with Trivy, and pushes to the Prod ACR. (The `frontend` service rebuilds the image entirely to embed production Entra Client IDs).
4. **GitOps Trigger**: Updates `values-prod.yaml` in the `main` branch of the Helm repo, triggering ArgoCD.
