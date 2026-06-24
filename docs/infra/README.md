# Infra UX Design

## Target Repository

`ceenkong-infra`

## Purpose

Infra UX covers setup, deployment, operations, and runbook experiences. It is not a consumer UI, but it still needs clear operator flows.

## Technology Constraints

Initial implementation should assume:

- Docker Compose
- `.env.example`
- shell scripts where needed
- README/runbook-driven operations
- optional reverse proxy
- optional MinIO

## Operator Flows

- first local/internal deployment
- environment configuration
- starting services
- checking service health
- viewing logs
- cleaning old artifacts
- backing up Postgres and object storage
- rotating signing material

## Design Requirements

Operator documentation and scripts should make state obvious:

- what is configured
- what is running
- what failed
- what data will be deleted
- what secrets are required

