# Autoscaling & Pod Disruption Budgets

[← Back to Kubernetes Architecture](Overview.md)

## Horizontal Pod Autoscaler (HPA)

Scalability is fully automated using Kubernetes Horizontal Pod Autoscaler (`hpa.yaml`).

- **Trigger**: Autoscaling dynamically triggers based on Resource Utilization. The average CPU target is set to `70%`.
- **Behavior**: The HPA defines an aggressive scale-up (0s stabilization window) and a conservative scale-down (300s stabilization window) to handle bursts effectively while preventing flapping.

## Pod Disruption Budgets (PDB)

**Finding:** There are **no** PDB resources defined in the Helm charts.
This presents a minor reliability risk during voluntary node disruptions (e.g., AKS node upgrades).
