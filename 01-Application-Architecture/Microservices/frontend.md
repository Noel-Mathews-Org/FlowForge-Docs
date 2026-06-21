# Frontend

[← Back to Microservices Catalog](README.md)

## Overview
- **Service Name**: `frontend`
- **Purpose**: Provides the user interface for the FlowForge platform.
- **Responsibilities**: Renders UI components, manages client-side state, and communicates with backend services via the API Gateway.

## Technical Details
- **Runtime**: Node.js
- **Framework**: Next.js 14.2
- **UI Libraries**: Tailwind CSS, Radix UI, Framer Motion, React Query, Lucide React
- **Ports**: 3000 (Internal), 80 (Ingress)
- **Deployment**: Exposed to the internet via Caddy reverse proxy and Azure Application Gateway.

## Dependencies
- **Incoming Calls**: Caddy Reverse Proxy (HTTPS 443)
- **Outgoing Calls**: API Gateway (`/api/*`)
- **External Dependencies**: Azure Entra ID (for OAuth redirects)

## Scaling Considerations
- Stateless frontend application; scales horizontally via Kubernetes HPA based on CPU utilization.
- Deployed using a blue-green strategy in Kubernetes to ensure zero-downtime updates.
