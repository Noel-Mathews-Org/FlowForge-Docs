# Monitoring

[← Back to Main Index](../README.md)

FlowForge utilizes Azure Monitor for all metric ingestion and dashboarding.

## Infrastructure Monitoring
- **AKS**: Container Insights are enabled, providing deep visibility into node memory, CPU, and pod-level health metrics.
- **PaaS**: Metrics for PostgreSQL, Redis, Key Vault, and Application Gateway are aggregated centrally.

## Application Monitoring
- All FastAPI and Next.js containers are instrumented with OpenTelemetry.
- Distributed tracing spans are pushed to Application Insights, allowing correlation between the frontend, the gateway, and downstream microservices.
