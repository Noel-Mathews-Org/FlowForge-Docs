# ADR-003 Gateway Authentication

## Context
Microservices require a reliable way to authenticate users and enforce RBAC without every service needing to re-implement token parsing, signature validation, and Entra ID integration.

## Decision
Authentication and token validation are offloaded entirely to the API Gateway. The Gateway verifies the JWT, extracts claims, and injects identity into `X-User-*` headers for downstream services to trust.

## Evidence
- `gateway/proxy.py` and `gateway/auth.py`.
- Downstream services utilizing `X-User-Role` for dependency injection.

## Benefits
- **Centralized Security**: Token validation logic exists in one place.
- **Performance**: Downstream services save CPU cycles by avoiding cryptographic JWT verification on every request.

## Tradeoffs
- **Trust Domain**: Downstream services must be heavily isolated. If an attacker bypasses the gateway and hits a microservice directly, they could forge `X-User-Role` headers and gain administrative access.

## Future Considerations
- Implement mTLS between the Gateway and microservices to guarantee that traffic arriving at the microservices definitively originated from the Gateway.
