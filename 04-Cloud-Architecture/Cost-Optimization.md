# Cost Optimization

[← Back to Cloud Architecture](Overview.md)

## Current Cost Controls
1. **Infracost Integration**: The CI/CD Terraform pipeline (`terraform-gitops.yml`) runs Infracost on pull requests, ensuring developers see the financial impact of infrastructure changes before merging.
2. **Horizontal Pod Autoscaling**: Application pods scale down aggressively based on CPU utilization, preventing over-provisioning of compute resources.
3. **Environment Parity**: The platform shares Terraform modules across `dev` and `prod`, allowing precise replication, but the dev environment scales down or uses lower SKUs where parameterized.

## Areas for Improvement
*See the [Architecture Gap Analysis](../14-Architecture-Review/Architecture-Gap-Analysis.md) for full recommendations on scaling down non-production environments out-of-hours.*
