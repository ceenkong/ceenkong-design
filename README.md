# CeenKong Design

CeenKong Design owns UI/UX design for **CeenKong APK Factory**.

Design work here must be implementation-aware. The goal is not to create abstract visual concepts that engineers must reinterpret. The goal is to provide design specs that match the technologies and boundaries of the implementation repositories.

Public review site:

- Pages URL: https://ceenkong.github.io/ceenkong-design/
- Static source: `site/`
- Deployment: `.github/workflows/pages.yml`

## Repository Map

```text
ceenkong-doc           System architecture, product planning, ADRs, cross-repo specs
ceenkong-design        This repository
ceenkong-platform      Django web portal and product UI implementation
ceenkong-engine        MobSF-based APK analysis and repackaging workers
ceenkong-infra         Docker Compose and deployment assets
```

## Design Scope

```text
docs/design-system.md       Shared product UI rules and components
docs/platform/              UI specs for ceenkong-platform
docs/engine/                UI presentation specs for engine outputs
docs/infra/                 Operator/deployment UX specs for infra workflows
site/                       Public static review pages and stage previews
```

## Start Here

All designer agents must read:

1. `AGENTS.md`
2. `ceenkong-doc/docs/architecture.md`
3. `docs/design-system.md`
4. The target area spec under `docs/platform/`, `docs/engine/`, or `docs/infra/`

## Current Status

Planning/scaffolding stage. Static review pages exist, but no design is implementation-ready until Stage 4 receives explicit owner approval.
