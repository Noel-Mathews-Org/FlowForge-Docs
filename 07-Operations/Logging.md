# Logging

[← Back to Main Index](../README.md)

All operational logs are centralized into an Azure Log Analytics Workspace provisioned in the Hub VNet.

## Diagnostic Settings
Terraform applies diagnostic settings to all PaaS resources to forward logs to the central workspace:
- **Application Gateway**: Access logs, WAF firewall logs.
- **Key Vault**: Audit events (who accessed which secret).
- **PostgreSQL**: Query logs and connection logs.
- **AKS**: Control plane audit logs.

## Application Logs
Standard output (`stdout`) and standard error (`stderr`) from all Kubernetes pods are captured automatically by the Azure Monitor agent (`ContainerLogV2`) and forwarded to Log Analytics.
