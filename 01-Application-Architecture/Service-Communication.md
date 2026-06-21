# Service Communication

[← Back to Application Architecture](Overview.md)

FlowForge utilizes a mix of synchronous HTTP APIs and asynchronous Redis Streams for inter-service communication.

## Synchronous Communication (HTTP)

Services communicate synchronously via HTTP for immediate data retrieval and actions. All internal service-to-service calls use a shared `INTERNAL_API_KEY` passed via the `X-Internal-Token` header for authentication.

### Known Service-to-Service HTTP Calls

| Source Service | Target Service | Path | Purpose |
|----------------|----------------|------|---------|
| Gateway | All Services | `/*` | Proxies external client requests with injected identity headers. |
| Project Service | Auth Service | `/internal/users/{id}` or `/internal/users/by-email` | Resolves user details before adding them to projects. |
| Project Service | Auth Service | `/internal/notifications` | Triggers in-app alerts for project events. |
| Task Service | Auth Service | `/internal/notifications` | Triggers in-app alerts for task assignments/approvals. |
| Task Service | Project Service | `/projects/{project_id}` | Dynamically fetches project manager details to notify them of an approval request. |
| Auth Service | Project Service | `/projects/internal/member-transferred` | Bulk-removes a member from their old manager's projects when their reporting manager changes. |
| Auth Service | Analysis Service| `/analytics/events` | Fetches a user's audit history for display. |

## Asynchronous Communication (Redis Streams)

For non-blocking operations, such as audit logging and email notifications, services publish events to a Redis Stream named `audit_log`.

See [Event Flows](Event-Flows.md) for more details.

## Considerations
- **Failure Handling**: Synchronous calls must handle timeouts and retries, especially if calling another service during a user-facing request.
- **Security**: The `INTERNAL_API_KEY` prevents unauthorized direct access to internal endpoints.
