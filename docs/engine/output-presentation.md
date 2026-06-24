# Engine Output Presentation Patterns

## Stage

Stage 1 of 4: Information architecture and user flow

## Target Repository

`ceenkong-engine`

## Target Implementation Consumers

- `ceenkong-platform` Django templates for task, analysis, and artifact pages
- HTMX/Alpine-style progressive refresh for long-running task status
- structured engine JSON outputs and log events consumed without a client-side SPA

## User Role

Internal release operator reviewing engine-generated analysis, build progress, failures, and artifacts inside the platform UI.

## Primary Workflow

```text
APK detail
-> analysis status summary
-> analysis report
-> build task detail
-> artifact detail
```

The engine does not own standalone screens. It owns structured outputs that let the platform move the operator through this workflow without forcing them to read raw worker logs first.

## Screen List

These are the platform surfaces that depend on engine output design:

1. `APK detail`
   Shows current analysis status, detected package metadata, latest verification state, and entry points to task history.
2. `Analysis report`
   Summarizes findings, notable warnings, and analysis artifacts in an operator-readable order.
3. `Build task detail`
   Shows pipeline step progression, current running step, failure category, retry context, and structured log stream.
4. `Artifact detail`
   Shows which output artifact is ready, how it relates to the source APK and build task, and whether signature verification passed.

## Navigation Relationships

```text
Project detail
  -> APK detail
     -> Analysis report
     -> Build task detail
        -> Artifact detail
```

Supporting relationships:

- `APK detail` links to the most recent analysis task and latest build task.
- `Analysis report` links back to the originating APK and forward to build configuration or build history.
- `Build task detail` links to the source APK, related build configuration, and any produced artifacts.
- `Artifact detail` links back to the build task that produced it.

## Information Groups Required From Engine

Stage 1 defines the information groups the platform must be able to present:

- task identity and overall status
- ordered pipeline steps with current step context
- analysis summary and findings rollup
- failure category and operator-facing failure location
- signature verification outcome
- artifact inventory and readiness
- structured log events scoped to task step and severity

## Implementation-Aware Constraints

- Engine responses should remain machine-readable first and presentation-friendly second.
- Platform pages assume server-rendered HTML with periodic or event-driven refresh, not a browser IDE or live client-side state store.
- Long-running work must preserve operator context: status summaries need compact fields that can refresh without replacing the whole page.
- Artifact and verification metadata must support audit-friendly detail pages rather than ad hoc download links.

## Open Questions And Contract Gaps

The current `../ceenkong-engine/docs/task-contract.md` is sufficient for coarse status, but Stage 1 review surfaced missing fields that the cross-repo contract likely needs before Stage 3:

1. Step timeline metadata
   Current contract defines a `step` string on log events, but not the ordered step list, per-step status, or timestamps needed for a stable task timeline.
2. Failure classification
   The contract mentions status and logs, but not a structured failure category, retryability flag, or operator-facing blame target such as tool, file, or config section.
3. Findings structure
   `findings summary` is too broad for platform navigation. The contract does not yet define severity, count buckets, or jump targets for detailed findings.
4. Verification detail
   The build output names `signature verification result`, but not the shape needed to explain why an artifact is or is not safe to download.
5. Artifact metadata
   The contract lists artifact paths, but not artifact type, display name, readiness state, or provenance fields required by the artifact detail flow.

## Owner Review Request

Review decision requested for Stage 1:

- approve this engine-output screen flow and navigation model for progression to Stage 2, or
- request changes before low-fidelity screen structure begins
