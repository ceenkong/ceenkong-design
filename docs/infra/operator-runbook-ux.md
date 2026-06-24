# Infra Operator UX And Runbook Format

## Stage

Stage 1 of 4: Information architecture and user flow

## Target Repository

`ceenkong-infra`

## Target Implementation Technology

- Docker Compose deployment assets
- `.env.example` and environment-specific `.env` files
- shell-script-assisted checks where useful
- README and runbook Markdown as the primary operator surface
- optional reverse proxy and MinIO documented as add-on paths, not alternate UI stacks

## User Role

Internal platform operator responsible for first deployment, environment setup,
service health checks, maintenance, and controlled secret rotation for
CeenKong APK Factory.

## Primary Workflow

```text
Deployment overview
-> environment configuration
-> preflight validation
-> start or update stack
-> verify service health
-> inspect logs and status
-> run maintenance playbook
```

This flow assumes the operator works from repository docs and terminal commands,
not a separate web console. The UX goal is to make runbook state legible and
auditable inside Markdown, shell output, and Compose service status.

## Screen List

These are documentation and operator surfaces that Stage 1 expects
`ceenkong-infra` to provide:

1. `Deployment overview`
   Entry page describing prerequisites, supported deployment modes, required
   secrets, and the decision tree for first deploy versus routine maintenance.
2. `Environment configuration`
   Explains which variables are mandatory, which are optional, where secrets
   come from, and which services each variable affects.
3. `Preflight validation`
   Lists the checks an operator runs before `docker compose up`, including file
   presence, secret availability, storage paths, host ports, and signing
   material location without exposing the secret values themselves.
4. `Stack start and update runbook`
   Defines the ordered steps to start services, upgrade an existing stack, and
   confirm the deployment reached a stable state.
5. `Health check and logs`
   Describes how operators confirm that Django, workers, Redis, Postgres, and
   object storage are healthy and where they inspect logs when a step fails.
6. `Maintenance playbook`
   Covers backup, artifact cleanup, and signing-material rotation as separate
   subflows with clear warnings about impact and reversibility.

## Navigation Relationships

```text
Deployment overview
  -> Environment configuration
  -> Preflight validation
     -> Stack start and update runbook
        -> Health check and logs
           -> Maintenance playbook
```

Supporting relationships:

- `Deployment overview` links directly to first deploy, update, and recovery
  entry points so operators do not scan the entire repository.
- `Environment configuration` links to the specific validation and deployment
  steps that depend on each variable group.
- `Health check and logs` links back to the deployment step that most likely
  caused the observed failure.
- `Maintenance playbook` links to backup prerequisites before cleanup or secret
  rotation actions begin.

## Runbook Format Direction

Stage 1 defines the operator experience shape, not the final command blocks.
Each runbook should eventually follow a stable format:

1. goal and operator role
2. prerequisites and required secrets
3. inputs and files touched
4. ordered actions
5. expected success signals
6. failure symptoms and first checks
7. rollback or recovery path

This format matches the architecture boundary: `ceenkong-infra` owns deployment
assets and runbooks, while `ceenkong-platform` and `ceenkong-engine` expose the
runtime signals those runbooks reference.

## Implementation-Aware Constraints

- The primary delivery surface is Markdown and terminal output, so navigation
  must survive plain GitHub rendering and local README viewing.
- Commands should be grouped by operator intent such as `validate`, `start`,
  `check`, `backup`, and `rotate`, rather than by low-level container details
  alone.
- Any status examples should use stable service names that match Docker Compose
  services and avoid private hostnames, customer identifiers, or real secrets.
- Secret handling steps must describe where values come from and how operators
  confirm presence without ever showing sensitive material in public previews.
- Optional components such as MinIO or a reverse proxy should appear as bounded
  branches off the main deployment path, not as separate architectures.

## Open Questions

1. Should `ceenkong-infra` expose a single operator entrypoint script for
   common validation and health checks, or should Stage 2 assume direct
   `docker compose` and shell commands only?
2. Does the owner want local single-host deployment and internal server
   deployment documented in one runbook tree or as separate top-level tracks?
3. Which maintenance actions must be reversible in the MVP beyond backup
   restore, especially for artifact cleanup and signing-material rotation?
4. Should signing-material rotation remain an infra-only runbook, or will it
   eventually need a platform-visible audit trail requirement in `ceenkong-doc`?

## Owner Review Request

Review decision requested for Stage 1:

- approve this infra operator flow and runbook navigation model for progression
  to Stage 2, or
- request changes before low-fidelity runbook structure begins
