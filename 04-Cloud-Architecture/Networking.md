# Cloud Networking

[← Back to Cloud Architecture](Overview.md)

FlowForge utilizes a Hub-and-Spoke network topology managed via Terraform.

## Hub Network (`vnet-<env>-hub`)
- **Purpose**: Centralized ingress, egress, and management.
- **Subnets**:
  - `AzureBastionSubnet`: Subnet for Azure Bastion services.
  - `snet-management`: Contains a Jumpbox virtual machine for administrative access.

## Spoke Network (`vnet-<env>-spoke`)
- **Purpose**: Hosts the workload resources and isolated PaaS connectivity.
- **Subnets**:
  - `snet-appgw`: Dedicated to the Application Gateway.
  - `snet-aks`: Houses the Azure Kubernetes Service (AKS) nodes.
  - `snet-pe`: Hosts Azure Private Endpoints (`private_endpoint_network_policies = "Enabled"`).
  - `snet-db`: Delegated specifically for PostgreSQL Flexible Server (`Microsoft.DBforPostgreSQL/flexibleServers`).

## VNet Peering
The Hub and Spoke VNets are peered, allowing traffic forwarding and gateway transit. The VPN Gateway in the Hub allows developers to connect and reach the Spoke resources natively.

## Network Security Groups (NSGs)
- `nsg-appgw-<env>`: Allows Internet inbound on 80/443 and 65200-65535 (GatewayManager).
- `nsg-aks-<env>`: Permits inbound TCP traffic (80, 443, 8080) specifically from `snet-appgw`.
- `nsg-pe-<env>`: Allows TCP 443 and 10000 strictly from the AKS subnet and the VPN Client Address Pool.
- `nsg-db-<env>`: Allows TCP 5432 strictly from the AKS subnet and VPN Client Address Pool.

## Egress & User Defined Routes (UDR)
A Route Table (`rt-spoke-<env>`) maps `0.0.0.0/0` (all outbound internet traffic) from the AKS, DB, and PE subnets to the private IP of the **Azure Firewall** located in the Hub. This ensures all egress traffic is inspected and logged.

## Private DNS
Private DNS Zones are linked to both the Hub and Spoke VNets for transparent resolution of Private Endpoints:
- `privatelink.vaultcore.azure.net`
- `privatelink.blob.core.windows.net`
- `privatelink.postgres.database.azure.com`
- `privatelink.redis.azure.net`
- `privatelink.<location>.azmk8s.io` (for the private AKS API server)

The Hub VNet acts as a DNS proxy, pointing custom DNS queries to the Azure Firewall's Private IP.
