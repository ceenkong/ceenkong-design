# Platform MVP Flow

Stage: 1 - Information Architecture and User Flow

This document is not an implementation-ready handoff. Per `docs/design-review-gates.md`, owner approval is required before adding low-fidelity screen structure, component details, or implementation acceptance criteria.

## Target Repository

`ceenkong-platform`

## Target Implementation Technology

- Django server-rendered templates for pages, forms, validation, permissions, and navigation.
- Django forms for upload, build configuration, and validation flows.
- HTMX for progressive refresh of task status, tables, logs, and artifact readiness where useful.
- Alpine-style JavaScript only for small local interactions such as disclosure panels and confirmation state.
- No React, Next.js, client-side routing, or browser IDE assumption for the MVP.

## User Role

Primary user: internal operator who uploads authorized APKs, starts analysis and build work, diagnoses task failures, and downloads generated artifacts.

The role needs an audit-friendly work surface that keeps project, APK, task, configuration, and artifact relationships visible without requiring direct access to engine workers or storage paths.

## Primary Workflow

```text
Dashboard
-> Project Detail
-> APK Detail
-> Build Config
-> Build Task Detail
-> Artifact Detail
```

Workflow intent:

- Start from a cross-project operational overview.
- Select a project and inspect its APK versions and recent work.
- Open a specific APK to understand analysis and workspace readiness.
- Create or open a repeatable build configuration for that APK.
- Follow the build task through queued, running, succeeded, failed, canceled, or expired states.
- Inspect and download the generated artifact only after it is ready and traceable.

## Screen List

Dashboard:

- Cross-project starting point for recent projects, running work, failures, and latest artifacts.

Project Detail:

- Project-scoped home for uploaded APK versions, build configurations, recent tasks, and artifacts.

APK Detail:

- APK-scoped inspection point for metadata, analysis status, decode workspace readiness, build configs, task history, and generated artifacts.

Build Config:

- Configuration workspace for repeatable APK build inputs tied to a source APK.

Build Task Detail:

- Task inspection view for long-running analysis or rebuild status, step timeline, logs, failures, and output artifact links.

Artifact Detail:

- Traceability and download view for generated files, signature verification state, source APK, build config, and build task.

## Navigation Relationships

Dashboard links to:

- Project Detail for each recent project.
- Build Task Detail for each recent or failed task.
- Artifact Detail for each latest ready artifact.

Project Detail links to:

- APK Detail for each uploaded APK.
- Build Config for each saved config.
- Build Task Detail for recent project tasks.
- Artifact Detail for generated project artifacts.

APK Detail links to:

- Project Detail as the parent context.
- Build Config for create/edit flows.
- Build Task Detail for analysis, decode, and build tasks.
- Artifact Detail for outputs generated from the APK.

Build Config links to:

- APK Detail as the parent source context.
- Build Task Detail after a build is enqueued.

Build Task Detail links to:

- Project Detail for project context.
- APK Detail for the source APK.
- Build Config for the build inputs when applicable.
- Artifact Detail after artifact collection succeeds.

Artifact Detail links to:

- Project Detail, APK Detail, Build Config, and Build Task Detail so the artifact remains traceable to its source and task history.

## Open Questions For Owner Review

- Should Stage 2 include a separate Project List screen, or should the Dashboard be the only project entry point for the first platform slice?
- Should Analysis Report be a separate MVP screen in this flow, or remain embedded inside APK Detail until analysis presentation gets its own design stage?
- Should Settings and signing-profile management stay outside this issue and be handled by a later platform flow?
- For the first owner-reviewed path, should upload and analysis be modeled as one guided flow or as separate actions from Project Detail and APK Detail?
- Which artifact types are in scope for the first UI pass: rebuilt APK only, or APK plus logs and metadata bundles?

## Stage 1 Review Request

Please review the information architecture and user flow before this design advances to Stage 2 low-fidelity screen structure.
