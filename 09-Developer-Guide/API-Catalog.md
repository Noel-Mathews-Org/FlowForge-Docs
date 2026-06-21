# API Catalog

[← Back to Main Index](../README.md)

This is a high-level catalog of the API surface area exposed by the Gateway. Detailed interactive documentation is available dynamically via Swagger UI (`/docs`) on each microservice.

## Public Endpoints
- `POST /api/auth/login` - Local authentication.
- `GET /api/auth/login/entra` - Initiate SSO.
- `GET /api/auth/callback/azure-ad` - SSO Callback.

## Protected Endpoints (Requires valid JWT)
### Identity (`/api/auth/*`)
- `GET /api/auth/me` - Get current user profile.
- `POST /api/auth/refresh` - Rotate JWT.

### Projects (`/api/projects/*`)
- `GET /api/projects` - List user's projects.
- `POST /api/projects` - Create a project (requires `manager` or `org_owner`).

### Tasks (`/api/tasks/*`)
- `GET /api/tasks?project_id=...` - List tasks.
- `POST /api/tasks` - Create task.
- `PATCH /api/tasks/{id}/status` - Update task status.
- `POST /api/tasks/{id}/approve` - Approve task (requires `manager`).

### AI & Analytics (`/api/analytics/*`, `/api/ai/*`)
- `GET /api/analytics/dashboard` - Get daily stats.
- `POST /api/ai/summarize/project/{id}` - Generate AI summary of project status.
