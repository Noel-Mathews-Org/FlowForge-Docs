# API Gateway

[← Back to Microservices Catalog](README.md)

## Overview
- **Service Name**: `gateway`
- **Purpose**: Acts as the single entry point for all internal API traffic from the frontend.
- **Responsibilities**: Enforces rate limiting, validates JWTs, extracts user claims, injects identity headers (`X-User-*`), and routes requests to appropriate downstream microservices.

## Technical Details
- **Runtime**: Python
- **Framework**: FastAPI
- **Ports**: 8000 (Internal)
- **Environment Variables**: `JWT_SECRET`, `REDIS_URL`

## Dependencies
- **Incoming Calls**: Frontend / Caddy (`/api/*`)
- **Outgoing Calls**: 
  - `auth-service`
  - `project-service`
  - `task-service`
  - `analysis-service`
- **Redis Dependencies**: Reads from Redis Cache to enforce IP and User-based rate limiting before proxying requests.

## Security Considerations
- **Boundary Defense**: This is the perimeter for the backend network. It drops invalid tokens before they consume internal compute resources.
- **Rate Limiting**: Protects downstream services from DoS attacks.
- **Trust Domain**: Downstream services inherently trust the `X-User-Role` injected by this gateway.
