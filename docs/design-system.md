# Design System

## Product Character

CeenKong APK Factory is an enterprise internal automation tool. The interface should feel calm, dense, operational, and audit-friendly.

Avoid marketing-style composition in the application itself. The MVP is a work surface, not a landing page.

## Visual Principles

- Use restrained contrast and clear hierarchy.
- Prefer compact tables, task timelines, forms, and logs.
- Use cards only for repeated objects or clearly framed panels.
- Avoid decorative gradients, abstract ornaments, and oversized hero sections inside the app.
- Keep status color meanings stable.

## Status Language

Recommended states:

- `queued`
- `running`
- `succeeded`
- `failed`
- `canceled`
- `expired`

Recommended severity:

- `info`
- `success`
- `warning`
- `error`

## Core Components

- Project table
- APK package table
- Build task table
- Task step timeline
- Structured log viewer
- Artifact list
- Build configuration form
- Analysis summary
- Finding list
- Empty state
- Error state
- Confirmation modal

## Interaction Principles

- Long-running work should never block the page.
- Build logs should stream or refresh without losing scroll context.
- Destructive actions require confirmation.
- Download actions should be visible only when artifacts are ready.
- AI suggestions must be reviewable before the user applies them.

## Accessibility Baseline

- Do not rely on color alone for status.
- Keep forms labeled.
- Preserve keyboard navigation for primary flows.
- Keep log text readable with monospace styling and adequate contrast.

