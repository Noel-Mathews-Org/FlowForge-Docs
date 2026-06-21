# Request Flow

[← Back to Application Architecture](Overview.md)

This document outlines the end-to-end request flows for typical user operations within the FlowForge platform.

## Standard API Request Flow

The following sequence diagram illustrates the path a standard API request takes from the user's browser through the infrastructure to the backend database.

```mermaid
sequenceDiagram
    actor User
    participant Browser as Frontend (Next.js)
    participant Caddy as Caddy Reverse Proxy
    participant Gateway as API Gateway
    participant Redis as Redis Cache
    participant Service as Backend Service (e.g. Task)
    participant DB as PostgreSQL DB

    User->>Browser: Interacts with UI
    Browser->>Caddy: HTTPS Request (/api/...)
    Caddy->>Gateway: Proxy Pass
    
    Gateway->>Redis: Check Rate Limit (IP/User)
    Redis-->>Gateway: Rate Limit OK
    
    Gateway->>Gateway: Validate JWT Signature & Expiry
    Gateway->>Gateway: Extract Claims (User-ID, Role)
    Gateway->>Gateway: Inject X-User Headers
    
    Gateway->>Service: Proxy Request + Headers
    
    Service->>Service: Enforce RBAC (require_role)
    Service->>DB: Execute Query (using X-User-ID)
    DB-->>Service: Return Data
    
    Service-->>Gateway: HTTP 200 JSON Response
    Gateway-->>Caddy: HTTP 200 JSON Response
    Caddy-->>Browser: HTTP 200 JSON Response
    Browser-->>User: Update UI
```

## Security & Failure Scenarios

- **Rate Limit Exceeded**: If Redis reports the rate limit is exceeded, the Gateway immediately returns `429 Too Many Requests`. The request never reaches the backend service.
- **Invalid JWT**: If the JWT is missing, expired, or invalid, the Gateway immediately returns `401 Unauthorized`.
- **Insufficient Role**: If the User role in `X-User-Role` is insufficient for the requested endpoint, the backend service returns `403 Forbidden`.
