# Platform UI Design

## Target Repository

`ceenkong-platform`

## Technology Constraints

Initial implementation should assume:

- Django server-rendered templates
- Django forms
- HTMX/Alpine-style progressive interactions where useful
- minimal JavaScript
- optional Monaco editor only after the core build pipeline works

Do not design a React/Next.js SPA unless `ceenkong-doc` changes the architecture first.

## MVP Screens

- Login
- Dashboard
- Project list
- Project detail
- APK detail
- Analysis report
- Build configuration
- Build task detail
- Artifact detail
- Settings

## Primary Flow

```text
login
-> create/select project
-> upload APK
-> view analysis status
-> configure build
-> run build
-> inspect logs
-> download artifact
```

## Review Gate

Platform UI design must follow `docs/design-review-gates.md`.

The first platform design pass should stop after Stage 1: Information Architecture and User Flow, then request owner review before adding low-fidelity screen structures.

## UI Priorities

- Make task state obvious.
- Keep build configuration repeatable.
- Make failures diagnosable.
- Keep artifacts traceable to source APK and build config.
- Avoid unnecessary customization in the MVP.
