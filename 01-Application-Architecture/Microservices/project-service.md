# Project Service

[← Back to Microservices Catalog](README.md)

## Overview
- **Service Name**: `project-service`
- **Purpose**: Manages organizational projects and their memberships.
- **Responsibilities**: CRUD operations for projects, assigning members, and tracking project lifecycle states (active, archived).

## Technical Details
- **Runtime**: Python
- **Framework**: FastAPI
- **Ports**: 8002 (Internal)
- **Environment Variables**: `DATABASE_URL`, `REDIS_URL`, `INTERNAL_API_KEY`

## Dependencies
- **Incoming Calls**: 
  - API Gateway (`/api/projects/*`)
  - Task Service (`/projects/{project_id}`)
  - Auth Service (`/projects/internal/member-transferred`)
- **Outgoing Calls**: 
  - Auth Service (`/internal/users/{id}`, `/internal/notifications`)
- **Database Dependencies**: PostgreSQL (`projects`, `project_members`)
- **Redis Dependencies**: Publishes to Redis Stream (`audit_log`) when events occur (e.g., `project_created`).

## Security Considerations
- Projects act as the primary isolation boundary for tasks. This service must correctly validate project ownership before modifying members.
