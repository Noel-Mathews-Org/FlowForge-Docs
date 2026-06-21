# Alerting

[← Back to Main Index](../README.md)

Alert rules are configured as code via Terraform (`modules/monitoring`).

## Active Alerts

| Resource | Condition | Action |
|----------|-----------|--------|
| Application Gateway | `TotalRequests` with `Http5xx` > 10 in 5 mins | Fire Alert |
| PostgreSQL | `cpu_percent` > 80% for 5 mins | Fire Alert |
| Redis | `used_memory_percentage` > 80% for 5 mins | Fire Alert |
| Key Vault | `Availability` < 100% | Fire Alert |

*To modify these thresholds, update the `monitoring` Terraform module and merge the PR.*
