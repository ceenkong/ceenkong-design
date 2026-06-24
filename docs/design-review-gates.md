# Design Review Gates

## Purpose

Design work must be checked with the project owner at clear stages. The designer should not produce a complete UI handoff in one pass without review.

Every stage must produce two artifacts:

- a durable Markdown spec in `docs/`
- a public static preview under `site/`, published to https://ceenkong.github.io/ceenkong-design/

The static preview is for fast owner review. The Markdown spec remains the engineering source of truth.

## Required Stages

### Stage 1: Information Architecture and User Flow

Output:

- target repository
- target implementation technology
- user role
- primary workflow
- screen list
- navigation relationships
- open questions

Review request:

- create or update a PR
- update the matching static page under `site/`
- add the `needs-user-review` label
- include the Pages URL
- ask the owner to approve the flow before moving to wireframe structure

Approval needed before Stage 2.

### Stage 2: Low-Fidelity Screen Structure

Output:

- screen-by-screen layout structure
- primary actions
- secondary actions
- key data shown on each screen
- empty/loading/error states at screen level

Review request:

- update the same PR or create a focused follow-up PR
- update the matching static page under `site/`
- add or keep the `needs-user-review` label
- include the Pages URL
- ask the owner to approve the screen structure before component details

Approval needed before Stage 3.

### Stage 3: Component and State Specification

Output:

- component list
- component behavior
- status and severity states
- form validation states
- table/list states
- log and artifact presentation rules

Review request:

- update the PR
- update the matching static page under `site/`
- include the Pages URL
- ask the owner to approve component/state behavior before implementation handoff

Approval needed before Stage 4.

### Stage 4: Implementation Handoff

Output:

- implementation-ready design spec
- target files or templates when known
- data dependencies
- interaction behavior
- acceptance criteria for implementation

Review request:

- ask the owner for final implementation approval
- update the final static preview under `site/`
- include the Pages URL
- only add `implementation-ready` after approval

Development agents may implement UI work only after this stage is approved.

## Label Rules

- `needs-user-review`: the owner must review before the designer continues.
- `user-approved`: the owner approved the current stage.
- `implementation-ready`: the final design handoff is approved for development.

## Explicit Approval Rule

Approval must be explicit in a PR comment, issue comment, or label. Silence, elapsed time, or a successful CI check is not approval.

## Automation Rule

Automated design agents must stop at each review gate. They may prepare the next questions, but they must not continue to the next stage until approval is present.

Automation must comment with:

- current stage
- PR URL
- Pages preview URL
- verification commands run
- exact owner decision requested
