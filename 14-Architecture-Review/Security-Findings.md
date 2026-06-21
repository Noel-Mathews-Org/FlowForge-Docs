# Security Findings

[← Back to Main Index](../README.md)

## Vulnerability Assessment

| Severity | Finding | Implication |
|----------|---------|-------------|
| **High** | Internal API Key shared across all microservices. | If an attacker breaches any single microservice, they obtain the `INTERNAL_API_KEY` and can impersonate any other service laterally. |
| **Medium** | Lack of mTLS between Gateway and Microservices. | Internal HTTP traffic is unencrypted. While isolated via Azure NSGs, a breached pod on the same AKS subnet could theoretically sniff internal traffic. |
| **Medium** | Redis Stream lacks authentication. | Any pod inside the cluster can publish to or consume from the `audit_log` stream, potentially leading to audit log spoofing. |
| **Low** | Checkov IaC scanning configured to `soft_fail: true`. | Minor infrastructure misconfigurations will not block the deployment pipeline, requiring manual review of logs to catch drift. |

*Note: Perimeter security, Workload Identity, and Key Vault integrations are implemented exceptionally well, demonstrating a mature zero-trust foundation.*
