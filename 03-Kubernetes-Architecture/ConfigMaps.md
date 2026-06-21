# ConfigMaps

[← Back to Kubernetes Architecture](Overview.md)

## Global Configuration Strategy

FlowForge avoids spreading configuration across multiple files. It utilizes a single, global ConfigMap defined at `Helm/templates/global-config.yaml` named `flowforge-config`.

## Injection Method
This ConfigMap is shared across all microservices using the `envFrom: configMapRef` logic within the Deployment templates. This injects all keys as environment variables into the pods.

## Contents
The global ConfigMap acts as the single source of truth for all non-sensitive configuration:
- Internal service URLs (e.g., `AUTH_SERVICE_URL`).
- Application Gateway references.
- Environment toggle flags.
- Azure Tenant IDs.
- Shared limits (e.g., `RATE_LIMIT_REQUESTS: 100`).
