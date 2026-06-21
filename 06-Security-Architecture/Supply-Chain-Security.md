# Supply Chain Security

[← Back to Security Architecture](Overview.md)

FlowForge secures its software supply chain through automated gates in the GitHub Actions pipeline (`_ci-reusable.yml`).

## Components
1. **SonarQube**: Performs Static Application Security Testing (SAST) on application source code to detect bugs, code smells, and vulnerabilities.
2. **Snyk**: Performs Software Composition Analysis (SCA) on Node.js (`package.json`) and Python (`requirements.txt`) dependencies to detect vulnerable third-party libraries.
3. **Checkov**: Scans Terraform code (`FlowForge-Terraform`) for infrastructure misconfigurations prior to deployment.
