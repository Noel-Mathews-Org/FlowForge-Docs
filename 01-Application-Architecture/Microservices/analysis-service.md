# Analysis Service

[← Back to Microservices Catalog](README.md)

## Overview
- **Service Name**: `analysis-service`
- **Purpose**: Provides AI-driven project analytics, reporting, and audit logging.
- **Responsibilities**: Aggregating audit logs from Redis streams, computing daily task metrics, and acting as an internal gateway to Azure AI Foundry / OpenAI for generating project summaries.

## Technical Details
- **Runtime**: Python
- **Framework**: FastAPI
- **Ports**: 8004 (Internal)
- **Environment Variables**: `DATABASE_URL`, `REDIS_URL`, `INTERNAL_API_KEY`, `AZURE_FOUNDRY_ENDPOINT`, `AZURE_KEYVAULT_URL`

## Dependencies
- **Incoming Calls**: 
  - API Gateway (`/api/analytics/*`, `/api/ai/*`)
  - Auth Service (`/analytics/events`)
- **Outgoing Calls**: 
  - Azure AI Foundry / OpenAI
- **Database Dependencies**: PostgreSQL (`audit_events`, `daily_task_stats`, `user_activity_stats`, `ai_usage_logs`)
- **Redis Dependencies**: Consumes from Redis Stream (`audit_log`) via a background worker (`workers/stream_consumer.py`).

## Scaling Considerations
- Processing high volumes of AI summarizations may require scaling this service independently or utilizing async job queues if OpenAI rate limits become a bottleneck.
