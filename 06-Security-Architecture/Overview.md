# Security Architecture Overview

[← Back to Main Index](../README.md)

FlowForge employs a defense-in-depth security architecture that spans identity management, network isolation, secret management, and supply chain security. The platform heavily relies on Azure Entra ID and zero-trust principles to secure both internal components and external access.

## Core Tenets
1. **Zero-Trust Networking**: All internal services require authentication and authorization.
2. **Secretless Infrastructure**: Use of Workload Identities and OIDC instead of long-lived secrets or connection strings.
3. **Automated Security Scanning**: Continuous scanning of code, containers, dependencies, and infrastructure as code during the CI/CD lifecycle.

## Navigation
* [Authentication](Authentication.md)
* [Authorization](Authorization.md)
* [Network Security](Network-Security.md)
* [Secret Management](Secret-Management.md)
* [Supply Chain Security](Supply-Chain-Security.md)
* [Container Security](Container-Security.md)
* [Compliance](Compliance.md)
