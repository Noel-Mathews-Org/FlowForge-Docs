# Cost Optimization Review

[← Back to Main Index](../README.md)

## Current Cost Drivers
Based on the Terraform deployment, the primary fixed costs are:
1. **Azure Firewall**: Deployed in the Hub VNet. Base fixed cost is high, regardless of throughput.
2. **Azure Application Gateway (WAF_v2)**: High fixed hourly cost.
3. **Azure Managed Redis (Enterprise)**: Enterprise tiers are significantly more expensive than standard caching tiers.
4. **AKS Cluster**: Standard Tier control plane incurs an hourly fee.

## Savings Opportunities

### Non-Production Environments
The `dev` environment utilizes the exact same infrastructure pattern as `prod`.
- **Recommendation 1**: Deploy a scaled-down architecture for development. Omit the Azure Firewall and Hub VNet entirely for `dev`, routing traffic directly out of the Spoke VNet via standard NAT Gateway.
- **Recommendation 2**: Use standard Redis cache for `dev` instead of Enterprise.
- **Recommendation 3**: Implement a cron job to scale the AKS node pools to `0` during nights and weekends for the `dev` environment.

### FinOps Integration
- The pipeline already utilizes `Infracost`. Enforce PR reviews for any Terraform change that increases costs by more than 10%.
