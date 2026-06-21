# Environment Variables

[← Back to Main Index](../README.md)

This is a comprehensive list of variables required to run the stack.

| Variable | Purpose | Location Used |
|----------|---------|---------------|
| `JWT_SECRET` | Signing token for identity | `gateway`, `auth-service` |
| `INTERNAL_API_KEY` | Bypassing Gateway for internal requests | All backend services |
| `DATABASE_URL` | PostgreSQL connection string | All backend services |
| `REDIS_URL` | Redis connection string | `gateway`, `project-service`, `task-service`, `analysis-service`, `worker` |
| `ENTRA_TENANT_ID` | Azure AD Tenant ID | `auth-service` |
| `ENTRA_CLIENT_ID` | App Registration Client ID | `auth-service`, `frontend` |
| `AZURE_FOUNDRY_ENDPOINT` | Azure AI API Endpoint | `analysis-service` |
| `SMTP_HOST` | Email server host | `auth-service`, `worker` |
