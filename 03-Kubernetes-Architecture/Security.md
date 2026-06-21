# Security Contexts

[← Back to Kubernetes Architecture](Overview.md)

## Pod Security Contexts

Containers across the FlowForge Helm deployments are configured to run as non-root users to limit the blast radius of any container compromise.

- `runAsUser: 1001`
- Privilege escalation is implicitly disabled by not running as UID 0.
