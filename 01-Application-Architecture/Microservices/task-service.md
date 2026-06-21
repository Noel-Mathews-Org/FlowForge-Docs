# Task Service

[← Back to Microservices Catalog](README.md)

## Overview
- **Service Name**: `task-service`
- **Purpose**: Manages kanban tasks, approval workflows, and comments.
- **Responsibilities**: Creating tasks, managing state transitions (TODO, IN_PROGRESS, DONE, BLOCKED), handling manager approval requests, and threaded commenting.

## Technical Details
- **Runtime**: Python
- **Framework**: FastAPI
- **Ports**: 8003 (Internal)
- **Environment Variables**: `DATABASE_URL`, `REDIS_URL`, `INTERNAL_API_KEY`

## Dependencies
- **Incoming Calls**: 
  - API Gateway (`/api/tasks/*`)
- **Outgoing Calls**: 
  - Project Service (`/projects/{project_id}`)
  - Auth Service (`/internal/notifications`)
- **Database Dependencies**: PostgreSQL (`tasks`, `approval_requests`, `task_comments`)
- **Redis Dependencies**: Publishes to Redis Stream (`audit_log`) when events occur (e.g., `task_created`, `status_changed`, `approval_requested`).

## Security Considerations
- Modifying tasks requires strict RBAC checks to ensure the user is an assigned member or a manager of the parent project.
