# AI Agent Instructions

This repository owns CeenKong UI/UX design. Treat `ceenkong-doc` as the source of truth for product and system architecture, and this repository as the source of truth for UI/UX decisions.

## Required Reading

Before changing files, read:

1. `README.md`
2. `ceenkong-doc/AGENTS.md`
3. `ceenkong-doc/docs/architecture.md`
4. `docs/design-system.md`
5. `docs/design-review-gates.md`
6. The target area spec
7. Current static review pages under `site/`

## Ownership Boundary

Owned here:

- design system
- layout rules
- screen flows
- component behavior
- empty/loading/error states
- data presentation patterns
- implementation-aware UI specifications

Not owned here:

- system architecture
- product roadmap
- web implementation code
- engine implementation code
- infra implementation code

## Implementation-Aware Design Rule

Every UI spec must name the target implementation repository and its expected technology constraints.

Initial constraints:

- `ceenkong-platform`: Django templates, server-rendered screens, HTMX/Alpine-style progressive interactions, optional Monaco later.
- `ceenkong-engine`: structured task logs, analysis findings, failure categories, and artifact metadata consumed by the platform UI.
- `ceenkong-infra`: Docker Compose setup, environment configuration, deployment status, and operator runbooks.

Do not introduce UI patterns that require a major framework shift without first adding an ADR in `ceenkong-doc`.

## Design Rules

- Prioritize dense, operational interfaces over marketing-style pages for the internal MVP.
- Keep screens task-focused: upload, analyze, configure, build, inspect logs, download artifacts.
- Do not design a browser IDE for the MVP.
- Define empty, loading, success, warning, failure, and permission states.
- Use consistent terminology with `ceenkong-doc`.
- Update design docs before implementation repositories change user-facing UI.
- Update `site/` whenever a design stage is ready for owner review.

## Public Pages Rule

`ceenkong-design` is public so the project owner can review static design output from any machine.

Designer agents must:

- keep static stage previews in `site/`
- publish reviewable output through GitHub Pages
- include the Pages URL in every design-stage PR or issue comment
- clearly mark whether the page is draft, needs owner review, user-approved, or implementation-ready
- avoid exposing secrets, APK files, signing material, customer data, or private implementation details in public pages

Do not treat a published Pages preview as approval. Approval still requires an explicit PR or issue comment, or the `user-approved` label.

## Owner Review Gates

UI/UX work must be reviewed with the project owner in stages. A designer agent may not move a design issue to the next stage until the current stage has a PR or issue comment explicitly asking for owner review.

Required stages:

1. Information architecture and user flow
2. Low-fidelity screen structure
3. Component and state specification
4. Implementation handoff

Rules:

- Add the `needs-user-review` label when a stage is ready for review.
- Update the matching static Pages preview before asking for review.
- Include the Pages URL in the review request.
- Do not add `implementation-ready` until the owner approves the implementation handoff stage.
- Do not assume silence means approval.
- Do not ask development agents to implement unapproved UI changes.
- If the owner requests changes, update the design and ask for review again.

## Safety Boundary

Do not design workflows for unauthorized third-party APK modification, malware deployment, credential theft, stealth, persistence, or abuse.
