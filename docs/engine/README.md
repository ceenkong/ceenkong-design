# Engine UI Presentation Design

## Target Repository

`ceenkong-engine`

## Purpose

The engine does not own a full user interface. It owns structured outputs that the platform UI must present clearly.

Design here defines how engine outputs should be shaped for UI consumption.

## Technology Constraints

Initial implementation should assume:

- Celery worker tasks
- structured JSON task outputs
- structured log events
- failure categories
- artifacts consumed by `ceenkong-platform`

## UI-Visible Engine Outputs

- task step timeline
- structured log events
- analysis summary
- findings list
- failure category
- signature verification result
- artifact metadata

## Presentation Requirements

Engine outputs should let the platform UI answer:

- Which step is running?
- Which step failed?
- Is this failure retryable?
- What file or tool caused the failure?
- Which artifact is safe to download?
- What can AI explain or summarize?

