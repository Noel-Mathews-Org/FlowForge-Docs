# Authentication

[← Back to Security Architecture](Overview.md)

Authentication in FlowForge is managed by the `auth-service` and enforced at the edge by the API `gateway`.

## Methods
- **Local Authentication**: Standard email and password hashes stored in PostgreSQL.
- **Single Sign-On (SSO)**: Integration with Azure Entra ID via OAuth2. 

## Flow
Regardless of the login method, the user receives an HS256 JWT containing their identity claims (`sub`, `email`, `role`, `org_id`).

For detailed sequence diagrams, see the [Application Authentication Flow](../01-Application-Architecture/Authentication-Flow.md).
