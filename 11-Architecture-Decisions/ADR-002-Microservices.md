# ADR-002 Microservice Architecture

## Context
FlowForge requires a scalable architecture allowing independent teams to build identity, project management, and AI analysis features without tightly coupling their release cycles.

## Decision
The platform was architected as a suite of backend microservices (`auth-service`, `project-service`, `task-service`, `analysis-service`) orchestrated behind a central API Gateway (`gateway`).

## Evidence
- `docker-compose.yml` defining isolated containers.
- `Helm/Chart.yaml` managing independent sub-charts.

## Benefits
- **Independent Scaling**: The `analysis-service` (AI intensive) can scale independently of the `project-service`.
- **Fault Isolation**: If the `notification-worker` crashes, the core API remains unaffected.
- **Technology Flexibility**: Allows different services to use different languages or frameworks in the future (though currently all are Python/FastAPI).

## Tradeoffs
- **Network Overhead**: Increased latency due to inter-service HTTP calls.
- **Data Consistency**: Lack of strict foreign keys across services requires eventual consistency mechanisms.

## Future Considerations
- Introducing a formal Service Mesh (e.g., Istio or Linkerd) to manage complex retries, mTLS, and observability between the microservices.
