# ADR-006 Hub and Spoke Networking

## Context
The Azure infrastructure requires enterprise-grade network segmentation, deep traffic inspection, and secure connectivity for developers, while ensuring no PaaS services are exposed to the public internet.

## Decision
A Hub-and-Spoke network topology was implemented using Terraform, centralizing egress through an Azure Firewall and placing all workloads into isolated subnets within the Spoke VNet.

## Evidence
- `FlowForge-Terraform/modules/hub_network` and `spoke_network`.
- `azurerm_private_endpoint` usage across data services.

## Benefits
- **Security**: Granular NSGs and UDRs ensure data cannot be exfiltrated easily.
- **Scalability**: New environments (e.g., staging, UAT) can be deployed as new Spokes peering back to the central Hub.

## Tradeoffs
- **Cost**: The Azure Firewall in the Hub represents a significant fixed monthly cost, even for low-traffic development environments.
- **Complexity**: Routing traffic through UDRs to a firewall requires careful DNS proxy configuration.
