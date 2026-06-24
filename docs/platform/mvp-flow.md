# Platform MVP Flow

## Target Implementation

Repository: `ceenkong-platform`

Technology constraints:

- Django server-rendered templates own page layout, forms, validation, permissions, and navigation.
- HTMX may refresh task status, tables, timelines, logs, and artifact links without replacing the full page.
- Alpine-style JavaScript may control small local interactions such as disclosure panels, upload progress affordances, and confirmation state.
- Do not assume React, Next.js, client-side routing, or a browser IDE for the MVP.

## Primary Journey

```text
Dashboard
-> Project Detail
-> APK Detail
-> Build Config
-> Build Task Detail
-> Artifact Detail
```

The platform should keep the flow auditable. Every generated artifact must link back to its `Project`, `ApkPackage`, `BuildConfig`, and `BuildTask`. Long-running work should expose the current pipeline step from the engine output rather than leaving users on a generic spinner.

## Shared Page Rules

- Place page title, status badge, key identifiers, and primary action in a compact header.
- Use compact tables for repeated objects: APK versions, build configs, tasks, logs, and artifacts.
- Status labels should follow the design system states: `queued`, `running`, `succeeded`, `failed`, `canceled`, and `expired`.
- Use color and text together for status. Do not rely on color alone.
- Prefer server-rendered empty, loading, warning, and failure partials that HTMX can swap into tables or panels.
- Keep destructive or irreversible actions behind confirmation modals.
- Show download actions only after the related artifact is ready and permission checks pass.

## Dashboard

Purpose:

- Give operators a dense overview of recent work across projects.
- Surface failed or blocked builds that need attention.
- Provide fast paths to recent projects, running tasks, and latest artifacts.

Data needed:

- Recent `Project` rows with project name, owner, latest APK, latest task status, and updated time.
- Recent `BuildTask` rows with project, APK package, current step, status, duration, and failure category when present.
- Failed build count grouped by failure category.
- Latest ready `Artifact` rows with artifact type, project, build task, signature verification result, and created time.

Primary actions:

- Create project.
- Open a project.
- Resume a running or failed build task.
- Open the latest artifact when it is ready.

Empty state:

- If no projects exist, show one compact panel that explains the first action and links to create a project.
- Hide recent task and artifact tables when there is no data, but keep their section headings visible with short empty messages.

Loading state:

- Render the page shell from Django immediately.
- Use HTMX polling for the recent task table while any task is `queued` or `running`.
- Preserve table sort and scroll position during partial refreshes.

Error state:

- If dashboard aggregates fail, render project navigation and replace the failed aggregate area with an error panel.
- Include a retry action that refreshes only the failed partial.
- For permission failures, omit unauthorized rows rather than rendering disabled links to hidden projects.

## Project Detail

Purpose:

- Manage one project's APK versions, build configurations, build history, and artifact trail.
- Make the next useful action obvious: upload an APK, inspect analysis, configure a build, or review failed output.

Data needed:

- `Project` metadata: name, owner, created time, updated time, retention policy summary when available.
- `ApkPackage` list with package name, version name, version code, upload time, analysis status, decode workspace status, and latest task.
- `BuildConfig` list with name, source APK, signing profile label, last used time, and validation state.
- Recent `BuildTask` rows scoped to the project.
- Ready `Artifact` rows scoped to the project.

Primary actions:

- Upload APK.
- Open APK detail.
- Create build configuration for a selected APK.
- Re-run or inspect recent task.
- Open artifact detail.

Empty state:

- If the project has no APKs, show upload as the primary action and hide build config and artifact tables behind concise empty messages.
- If APKs exist but no build configs exist, show a secondary prompt to create the first config from the latest analyzed APK.

Loading state:

- APK upload uses a Django form post. The page should return with either validation errors or the new APK row.
- HTMX may refresh the APK table and recent task table while analysis or decode tasks run.
- The upload action remains visible, but duplicate submit should be disabled while the request is active.

Error state:

- Form validation errors stay inline next to fields.
- Failed APK analysis should appear as a row status with a link to the related task detail.
- Storage or permission failures render a page-level error panel while preserving project metadata where possible.

## APK Detail

Purpose:

- Inspect one uploaded APK and decide whether to analyze, configure, rebuild, or troubleshoot it.
- Keep analysis and decode state close to the available build actions.

Data needed:

- `ApkPackage` metadata: file name, package name, version name, version code, SHA-256, size, upload actor, upload time, and storage status.
- Latest `AnalysisReport` summary with severity counts and analysis status.
- `Workspace` status for decoded sources when available.
- Build configs tied to this APK.
- Task history for analysis, decode, and build tasks.
- Artifact list generated from this APK.

Primary actions:

- Start analysis when no report exists.
- Open analysis report when available.
- Create build configuration.
- Open existing build configuration.
- Inspect task detail for failed or running work.

Empty state:

- If analysis has not run, show a compact status panel with a single start analysis action.
- If decode workspace is not ready, show the dependency clearly and disable build creation with explanatory text.
- If no build configs exist after analysis succeeds, offer create build config as the primary next action.

Loading state:

- Use HTMX polling for analysis and decode status panels while related tasks are active.
- Show the current engine step name when available, such as `static_analyze` or `decode`.
- Do not replace the whole page during task refreshes.

Error state:

- Failed analysis or decode states link to `BuildTask` detail or the relevant task detail view.
- Missing source file or expired workspace states should describe the operational issue and show the available recovery action, such as re-upload or re-run analysis.
- Permission failures should return the user to project detail with a concise error message.

## Build Config

Purpose:

- Define repeatable, reviewable APK build inputs before a rebuild task starts.
- Make configuration risk visible before the user enqueues work.

Data needed:

- Source `Project` and `ApkPackage` identifiers.
- Existing `BuildConfig` values when editing: app name, icon replacement metadata, channel identifier, manifest metadata, endpoint replacement rules, and signing profile.
- Available signing profile labels and their safe display metadata. Do not expose keys or private certificate data.
- Analysis summary signals that affect config choices, such as package name and manifest metadata keys.
- Validation errors from Django forms.

Primary actions:

- Save draft config.
- Validate config.
- Start build after validation succeeds.
- Cancel and return to APK detail.

Empty state:

- For a new config, prefill safe source APK metadata where possible and leave optional replacement fields blank.
- If no signing profile is available, keep the form editable but disable start build until a profile is selected or configured.

Loading state:

- Standard Django form submission handles save and validation.
- HTMX may validate endpoint replacement rows or manifest metadata rows inline, but server validation remains authoritative.
- While starting a build, disable the submit button and return the user to the new build task detail after enqueue succeeds.

Error state:

- Field-specific validation errors render inline.
- Cross-field errors appear above the form in a warning panel, for example an endpoint rule without a target value.
- Signing profile permission or availability errors must not reveal secret material. Show only the profile label and operational status.

## Build Task Detail

Purpose:

- Show long-running analysis and rebuild task progress in a way that is diagnosable and audit-friendly.
- Help users distinguish transient failures from configuration or signing failures.

Data needed:

- `BuildTask` metadata: task type, status, created time, started time, finished time, duration, actor, source APK, build config, and retry count.
- Engine pipeline step results in order: `validate_input`, `static_analyze`, `decode`, `prepare_workspace`, `apply_config_patch`, `rebuild`, `zipalign`, `sign`, `verify_signature`, and `collect_artifacts` when applicable.
- Structured `BuildLog` events with timestamp, severity, step, message, tool, and file path when safe to display.
- Failure category, retryability, and concise explanation.
- Related artifacts when ready.

Primary actions:

- Refresh or auto-refresh active task state.
- Retry when the failure category is retryable and permissions allow it.
- Open source APK.
- Open build config.
- Open ready artifact.
- Copy a filtered log excerpt for support without exposing secrets.

Empty state:

- If logs have not arrived yet for a queued task, show queued status and the expected first step.
- If no artifacts exist because the task is not complete, keep the artifact section visible with an empty message.

Loading state:

- HTMX polls the status header, step timeline, log viewer, and artifact list while the task is `queued` or `running`.
- The log viewer should append or refresh without losing scroll context.
- Stop polling when the task enters `succeeded`, `failed`, `canceled`, or `expired`.

Error state:

- Failed steps are highlighted in the timeline with severity text and failure category.
- The error explanation panel should show what failed, whether retry is allowed, and the next useful action.
- If log loading fails independently, keep task status visible and show a retry action for only the log panel.

## Artifact Detail

Purpose:

- Inspect generated files before download and preserve traceability to the source APK, build config, and task.
- Make signature verification and artifact readiness explicit.

Data needed:

- `Artifact` metadata: type, file name, size, SHA-256, created time, retention or expiration state, and storage location label.
- Source `Project`, `ApkPackage`, `BuildConfig`, and `BuildTask` links.
- Signature verification result from the engine.
- Download permission state.

Primary actions:

- Download artifact when ready.
- Open source APK.
- Open build config.
- Open build task detail.
- Copy checksum.

Empty state:

- Artifact detail should not normally exist without an artifact record.
- If the file is still being collected, show pending readiness and link back to the task detail.

Loading state:

- HTMX may refresh readiness and signature verification panels while `collect_artifacts` or `verify_signature` is still running.
- Keep checksum and download areas hidden until the artifact is complete.

Error state:

- If the artifact file is missing from storage, show the metadata that remains and disable download.
- If signature verification failed, keep download disabled by default and link to the task logs for diagnosis.
- If permission checks fail, return a forbidden state without exposing storage paths.
