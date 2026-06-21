# Security Scanning

[← Back to CI/CD Architecture](Overview.md)

FlowForge incorporates automated security scanning at multiple stages of the deployment lifecycle.

## Static Application Security Testing (SAST)
- **Tool**: SonarQube
- **Integration**: Runs during the `_ci-reusable.yml` pipeline. Scans source code for bugs, code smells, and vulnerabilities before the container is built.

## Software Composition Analysis (SCA)
- **Tool**: Snyk
- **Integration**: Runs during the `_ci-reusable.yml` pipeline. Scans Node.js (`package.json`) and Python (`requirements.txt`) dependencies for known vulnerabilities and registers the project for continuous monitoring.

## Container Scanning
- **Tool**: Trivy
- **Integration**: 
  - Scans newly built development images for OS and library vulnerabilities (`CRITICAL` and `HIGH` severities) before pushing to ACR.
  - Scans production candidate images prior to pushing them to the production registry in `prod-release.yml`.

## Infrastructure as Code (IaC) Scanning
- **Tool**: Checkov
- **Integration**: Runs on the Terraform configuration during the CI stage (`terraform-gitops.yml`) to catch cloud misconfigurations. Configured with `soft_fail: true` to prevent blocking deployments for minor issues while still surfacing warnings.
