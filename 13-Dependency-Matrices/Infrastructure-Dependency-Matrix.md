# Infrastructure Dependency Matrix

[← Back to Main Index](../README.md)

| Resource | Deployed Into (VNet/Subnet) | Depends On | Authentication Method |
|----------|-----------------------------|------------|-----------------------|
| Azure Firewall | Hub VNet (`AzureFirewallSubnet`) | Hub VNet | N/A |
| VPN Gateway | Hub VNet (`GatewaySubnet`) | Hub VNet | Entra ID P2S |
| Application Gateway | Spoke VNet (`snet-appgw`) | Public IP, WAF Policy | N/A |
| AKS Cluster | Spoke VNet (`snet-aks`) | AppGW, Route Table | Managed Identity (`mi-<name>-cp`) |
| Key Vault | Spoke VNet (`snet-pe`) | Private DNS Zone | Entra ID RBAC |
| Redis | Spoke VNet (`snet-pe`) | Private DNS Zone | Entra ID RBAC / Access Keys |
| PostgreSQL | Spoke VNet (`snet-db`) | Private DNS Zone | Entra ID / Local Auth |
| Storage Account | Spoke VNet (`snet-pe`) | Private DNS Zone | Entra ID RBAC |
