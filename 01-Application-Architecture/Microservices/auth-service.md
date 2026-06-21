# Auth Service

[← Back to Microservices Catalog](README.md)

## Overview
- **Service Name**: `auth-service`
- **Purpose**: Manages platform identity and access control.
- **Responsibilities**: User registration, authentication (local and Entra ID), JWT minting, role management, invitation handling, and Entra ID security group synchronization.

## Technical Details
- **Runtime**: Python
- **Framework**: FastAPI
- **Ports**: 8001 (Internal)
- **Environment Variables**: `DATABASE_URL`, `JWT_SECRET`, `INTERNAL_API_KEY`, `ENTRA_TENANT_ID`, `ENTRA_CLIENT_ID`, `ENTRA_CLIENT_SECRET`, `ENTRA_GROUP_MANAGER`, `SMTP_HOST`, etc.

## Dependencies
- **Incoming Calls**: 
  - API Gateway (`/api/auth/*`)
  - Project Service (`/internal/users/*`)
  - Task Service (`/internal/notifications`)
- **Outgoing Calls**: 
  - Project Service (`/projects/internal/member-transferred`)
  - Analysis Service (`/analytics/events`)
- **Database Dependencies**: PostgreSQL (`users`, `invitations`, `notifications`, `refresh_tokens`)
- **External Dependencies**: 
  - Azure Entra ID (Graph API)
  - SMTP Server

## Security Considerations
- Stores sensitive password hashes and mints identity tokens.
- Directly manipulates enterprise Azure AD groups based on internal role changes.
