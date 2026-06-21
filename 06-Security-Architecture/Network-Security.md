# Network Security

[← Back to Security Architecture](Overview.md)

The network is heavily segmented utilizing an Azure Hub-and-Spoke topology.

## Perimeter Defense
- **Azure Application Gateway WAF**: Inspects inbound traffic against OWASP 3.2 rulesets in `Prevention` mode.
- **Azure Firewall**: Inspects and filters all outbound egress traffic from the AKS cluster.

## Internal Isolation
- **Private Endpoints**: PaaS services (Key Vault, Storage, Redis) disable public network access and expose IPs internally via `snet-pe`.
- **Subnet Delegation**: PostgreSQL is isolated into its own delegated subnet (`snet-db`).
- **Network Security Groups (NSGs)**: Applied strictly to each subnet to drop unauthorized traffic (e.g., PostgreSQL port 5432 is only accessible from the AKS nodes or the VPN Gateway).

For a detailed visual map, see [Cloud Traffic Flow](../04-Cloud-Architecture/Traffic-Flow.md).
