# Container Security

[← Back to Security Architecture](Overview.md)

## Vulnerability Scanning
All Docker containers are scanned using **Trivy** during the CI/CD pipeline.
- Builds are halted if `CRITICAL` or `HIGH` vulnerabilities are found in OS packages or libraries.
- Scanning occurs both after initial build (dev) and prior to production promotion (prod).

## Runtime Security
- Containers are configured to run as a non-root user (`runAsUser: 1001`), minimizing privilege escalation risks if a container is breached.
- The `azure` CNI combined with `calico` allows for granular network policies inside the cluster (though currently relying primarily on NSG-level isolation).
