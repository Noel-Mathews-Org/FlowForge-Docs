# Notification Worker

[← Back to Microservices Catalog](README.md)

## Overview
- **Service Name**: `notification-worker`
- **Purpose**: Dispatches asynchronous email notifications based on system events.
- **Responsibilities**: Reliable delivery of SMTP emails for events like task assignments, approval requests, and project membership changes.

## Technical Details
- **Runtime**: Python
- **Framework**: Standalone script (`worker.py`)
- **Environment Variables**: `REDIS_URL`, `SMTP_HOST`, `SMTP_PORT`, `SMTP_USERNAME`, `SMTP_PASSWORD`

## Dependencies
- **Incoming Calls**: None (Pulls data instead of receiving requests)
- **Redis Dependencies**: Consumes from Redis Stream (`audit_log`) via a consumer group.
- **External Dependencies**: SMTP Server

## Failure Scenarios
- If the SMTP server is down, the worker will fail to process the message and it will remain in the Redis Pending Entries List (PEL) for retry.
