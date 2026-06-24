# Platform MVP Screens

## Dashboard

Purpose:

- show recent projects
- show recent build tasks
- highlight failed builds
- link to latest artifacts

Primary components:

- recent task table
- failed task callout
- artifact shortcuts

## Project Detail

Purpose:

- manage APKs and build history for one project

Primary components:

- APK version table
- build template list
- recent task list
- upload action

## APK Detail

Purpose:

- inspect one uploaded APK and start analysis/build actions

Primary components:

- APK metadata panel
- analysis status panel
- decode workspace status
- available build configs
- task history

## Build Configuration

Purpose:

- define repeatable, configuration-driven APK modifications

Fields:

- app name
- icon replacement
- channel identifier
- manifest meta-data
- endpoint replacement rules
- signing profile

## Build Task Detail

Purpose:

- inspect long-running build state and failures

Primary components:

- status header
- step timeline
- structured log viewer
- error explanation
- artifact links

## Artifact Detail

Purpose:

- inspect and download generated artifacts

Primary components:

- artifact metadata
- source APK link
- build config link
- signature verification result
- download action

