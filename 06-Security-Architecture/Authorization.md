# Authorization

[← Back to Security Architecture](Overview.md)

Authorization relies on strict Role-Based Access Control (RBAC).

## Roles
- `platform_admin`
- `org_owner`
- `manager`
- `member`

## Enforcement
The API Gateway extracts the `role` claim from the JWT and injects it into the `X-User-Role` HTTP header. Downstream microservices enforce authorization via dependency injection, requiring specific roles for specific endpoints.

## Synchronization
The `auth-service` maps internal roles to Entra ID Security Groups via Microsoft Graph API, ensuring external IT administrators have a single pane of glass for organizational permissions.

For detailed mappings, see the [Application Authorization Flow](../01-Application-Architecture/Authorization-Flow.md).
