# Operational Findings

[← Back to Main Index](../README.md)

## Operations Assessment

| Area | Finding | Impact |
|------|---------|--------|
| **Monitoring Coverage** | Deeply instrumented with OpenTelemetry and Azure Monitor. | High visibility into application tracing and latency. |
| **Alerting Coverage** | Basic metric alerts configured (CPU, Memory, 5xx errors). | Sufficient for catching major outages, but lacks synthetic monitoring (uptime checks) to ensure the frontend is actually usable. |
| **Backup Coverage** | Automated Azure PaaS backups utilized for Postgres. | Standard 7-day retention is likely applied, but not explicitly codified in Terraform. No evidence of Redis snapshotting. |
| **Runbook Coverage** | Basic runbooks exist. | High dependence on Azure Portal for disaster recovery (e.g., restoring deleted databases). |
