# Deployments & Workloads

[← Back to Kubernetes Architecture](Overview.md)

## Stateless Deployments
Every microservice in FlowForge is deployed as a standard Kubernetes `Deployment` resource. 
There are **no StatefulSets** in the cluster.

## Deployment Configurations
- **Security Context**: Containers run as a non-root user (`runAsUser: 1001`).
- **Observability**: OpenTelemetry metrics and traces are automatically injected.
- **Labels & Selectors**: Utilize standard Helm chart labels (`app.kubernetes.io/name`, `app.kubernetes.io/instance`).

## Frontend Blue-Green Anomaly
The `frontend` chart contains an anomaly in its configuration (`Helm/charts/frontend/templates/deployment.yaml`). It defines a `strategy: blueGreen` block under a `kind: Deployment`. 

*Note: Native Kubernetes Deployments do not recognize a blueGreen strategy block. This suggests the presence of a mutation webhook (like Argo Rollouts) that translates this resource, or it is a configuration error.*
