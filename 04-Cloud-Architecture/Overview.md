# Cloud Architecture Overview

[← Back to Main Index](../README.md)

FlowForge is deployed natively on Microsoft Azure, utilizing Infrastructure as Code (Terraform) for provisioning. The architecture relies on a Hub-and-Spoke network topology, prioritizing network isolation and Managed Identities for secure access.

## Architecture Highlights
1. **Network Isolation**: All PaaS services (Database, Redis, Key Vault, Storage) are deployed without public endpoints, utilizing Private Endpoints injected into a spoke VNet.
2. **Zero-Trust Identity**: Heavy reliance on Azure Entra ID (Azure AD), User-Assigned Managed Identities, and Workload Identity federation.
3. **Zone Redundancy**: Infrastructure is deployed across multiple availability zones (Zones 2 & 3) where supported.

## Related Documents
* [Networking](Networking.md)
* [Identity & RBAC](Identity.md)
* [Security & Encryption](Security.md)
* [Compute Layer (AKS)](Compute.md)
* [Data Services](Data-Services.md)
* [Traffic Flow](Traffic-Flow.md)
* [Reliability & High Availability](Reliability.md)
* [Cost Optimization](Cost-Optimization.md)
* [Disaster Recovery](Disaster-Recovery.md)
