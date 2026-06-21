# Local Development

[← Back to Main Index](../README.md)

The FlowForge application repository is designed to be run locally via `docker-compose.yml`.

## Prerequisites
- Docker & Docker Compose
- Node.js (for frontend development)
- Python 3.11+ (for backend development)

## Running the Stack
1. Clone the `FlowForge` repository.
2. Copy `.env.example` to `.env` and fill in the required values (see [Environment Variables](Environment-Variables.md)).
3. Run `docker-compose up -d --build`.

This will start:
- Caddy reverse proxy on `localhost:80`
- PostgreSQL on `localhost:5432`
- Redis on `localhost:6379`
- Frontend on `localhost:3000`
- All backend services

You can access the application at `http://localhost`.
