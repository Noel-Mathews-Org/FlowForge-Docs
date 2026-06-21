# Traffic Flow

[← Back to Cloud Architecture](Overview.md)

This document visualizes the exact network hops for inbound and outbound traffic.

## Inbound Traffic (Internet to Service)

```mermaid
flowchart TD
    Internet([Internet User]) -->|HTTPS:443| AppGW[Azure App Gateway WAF]
    AppGW -->|HTTP:80 via snet-appgw| Ingress[AKS AGIC Ingress]
    Ingress -->|HTTP:80 via snet-aks| GatewaySVC[Gateway Service]
    GatewaySVC -->|HTTP:8001| AuthSVC[Auth Service]
    AuthSVC -->|TCP:5432 via snet-db| DB[(PostgreSQL Flexible Server)]
```

## Outbound Traffic (Service to Internet)

To ensure data exfiltration prevention, AKS nodes cannot reach the internet directly. 

```mermaid
flowchart TD
    AKS[AKS Pod] -->|HTTPS:443| RouteTable[rt-spoke-env UDR]
    RouteTable -->|next_hop: VirtualAppliance| Firewall[Azure Firewall in Hub]
    Firewall -->|Rule Evaluation| Rule{Allow?}
    Rule -->|Yes| Internet([External API e.g. Entra ID])
    Rule -->|No| Drop([Drop Packet])
```
*Note: The Firewall explicitly allows egress to AKS required FQDNs and NTP services.*

## Developer VPN Traffic

```mermaid
flowchart TD
    Dev([Developer]) -->|OpenVPN TLS| VPNGW[VPN Gateway in Hub]
    VPNGW -->|VNet Peering| SpokeVnet[vnet-env-spoke]
    SpokeVnet -->|TCP:5432| DB[(PostgreSQL)]
```
