# CI/CD Architecture Overview

[← Back to Main Index](../README.md)

FlowForge utilizes a decentralized CI/CD strategy powered entirely by GitHub Actions and ArgoCD.

## Principles
1. **Reusable Pipelines**: A central `.github` repository stores standard CI templates to ensure compliance and security scanning consistency across all microservices.
2. **GitOps**: Deployments to the Kubernetes cluster are pulled rather than pushed. ArgoCD monitors the `FlowForge-Helm` repository and applies changes automatically.
3. **Continuous Security**: Every pull request and merge is scanned by SonarQube (SAST), Snyk (SCA), Trivy (Container scanning), and Checkov (IaC scanning).

## Navigation
* [GitHub Actions Workflows](GitHub-Actions.md)
* [Application Pipelines](Application-Pipeline.md)
* [Terraform Pipelines](Terraform-Pipeline.md)
* [Security Scanning](Security-Scanning.md)
* [GitOps & ArgoCD](GitOps.md)
