# Scalability Findings

[← Back to Main Index](../README.md)

## Scalability Assessment

| Area | Observation | Capability |
|------|-------------|------------|
| Compute Scaling | The Horizontal Pod Autoscaler (HPA) is configured with aggressive scaling targets (70% CPU). | **High**: The stateless architecture allows pods to scale rapidly. |
| Node Scaling | AKS VMSS node pools are utilized. | **High**: The underlying infrastructure will automatically provision new VMs when pods cannot be scheduled. |
| Database Scaling | PostgreSQL Flexible Server deployed. | **Medium**: Read-heavy workloads will eventually bottleneck the single primary node. Read Replicas are not currently configured. |
| Event Processing | `notification-worker` consumes from Redis Streams. | **High**: Redis Streams supports Consumer Groups, allowing horizontal scaling of the worker pods to handle massive event backlogs. |
