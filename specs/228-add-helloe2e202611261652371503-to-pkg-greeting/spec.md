# Feature Specification: Add HelloE2E202611261652371503 to pkg/greeting (e2e cross-repo spawn)

**Feature Branch**: `fabrik/issue-228`
**Created**: 2026-11-26
**Status**: Specified
**Input**: User description: "

> ## Problem
>
> The `pkg/greeting` package needs a new exported function that returns a per-run-unique token for the `20261126-165237-1503` cross-repo spawn e2e run. Another repository (`handarbeit/fabrik-test-gamma`) will call this function after this change has landed, so the function must exist on this repo's default branch first.
>
> ## Requirements
>
> Use the function name and string literal below exactly as written. Do not alter, abbreviate or regenerate them. The scenario's final assertion looks for this exact text in this repository.
>
> - **FR-001**: In `pkg/greeting/greeting.go`, add the exported function `HelloE2E202611261652371503() string`. It returns exactly the literal `"e2e-cross-repo-spawn-20261126-165237-1503"`.
> - **FR-002**: Give it a doc comment that follows the existing convention in the file: `// HelloE2E202611261652371503 returns the run-unique token for the 20261126-165237-1503 cross-repo spawn e2e run.`
> - **FR-003**: In `pkg/greeting/greeting_test.go`, add `TestHelloE2E202611261652371503(t *testing.T)`. It uses `const want = "e2e-cross-repo-spawn-20261126-165237-1503"` and fails if `HelloE2E202611261652371503()` returns anything else.
> - **FR-004**: Append the new function and test after the existing ones. Do not reorder, rename or remove existing functions or tests. Other e2e runs may append to the same files, so on a rebase conflict keep every other run's functions and tests.
>
> ## Scope
>
> - Only `pkg/greeting/greeting.go` and `pkg/greeting/greeting_test.go` change.
> - Do not modify any other behavior in this repo.
> - No new dependencies. The module stays at `go 1.22`.
> - If `pkg/greeting` does not exist, create the minimum needed (package `greeting`, the function and its test).
>
> ## Acceptance
>
> - `HelloE2E202611261652371503` exists on this repo's default branch and returns `"e2e-cross-repo-spawn-20261126-165237-1503"`.
> - `go test ./...` passes in this repo, including the new test.
> - The literal `e2e-cross-repo-spawn-20261126-165237-1503` appears in this repo's default branch after the change is merged.
>
> ## Background
>
> This is an end-to-end regression scenario for cross-repo decomposition, covering #803 (on-demand spawn-target repo init) and #1308 (per-run-unique fixture token). The per-run token is baked into the names above so that artifacts from earlier runs cannot satisfy this run's check."

## Background

This issue is one part of an end-to-end regression scenario for cross-repo decomposition. It covers #803 (on-demand spawn-target repo init) and #1308 (per-run-unique fixture token). The per-run token is baked into the names so that artifacts from earlier runs cannot satisfy this run's check.

Another repository, `handarbeit/fabrik-test-gamma`, will call the new function after this change has landed. The function must therefore exist on this repository's default branch first. The scenario's final assertion looks for the exact token text in this repository, so the identifiers and the literal must be used exactly as given and never abbreviated or regenerated.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Provide the run-unique token function (Priority: P1)

A consumer of `pkg/greeting`, in this repo or in `handarbeit/fabrik-test-gamma`, can call `HelloE2E202611261652371503()` and receive the run-unique token `"e2e-cross-repo-spawn-20261126-165237-1503"`.

**Why this priority**: This is the whole deliverable. The cross-repo scenario cannot proceed until the function exists on the default branch.

**Independent Test**: Run `go test ./...` in this repo. The new `TestHelloE2E202611261652371503` passes. Searching the default branch for the token literal finds it.

**Acceptance Scenarios**:

1. **Given** the change is merged to the default branch, **When** `HelloE2E202611261652371503()` is called, **Then** it returns exactly `"e2e-cross-repo-spawn-20261126-165237-1503"`.
2. **Given** the change is merged to the default branch, **When** the repository is searched for `e2e-cross-repo-spawn-20261126-165237-1503`, **Then** the literal is found.
3. **Given** the new function returned any other value, **When** `go test ./...` runs, **Then** `TestHelloE2E202611261652371503` fails.

---

### User Story 2 - Coexist with other runs' additions (Priority: P2)

Other e2e runs may append their own functions and tests to the same two files. The new function and test must be appended without disturbing anything already there.

**Why this priority**: Parallel runs touch the same files. Losing another run's function would break that run's own assertions.

**Independent Test**: After the change, every function and test that existed before is still present, unrenamed and in its original order. The new ones come after them.

**Acceptance Scenarios**:

1. **Given** existing functions and tests in the two files, **When** the change is applied, **Then** all of them remain unchanged and in the same order, with the new function and test appended after them.
2. **Given** a rebase conflict with another run's additions, **When** it is resolved, **Then** every other run's functions and tests are kept alongside the new ones.

---

### Edge Cases

- `pkg/greeting` does not exist: create the minimum needed (package `greeting`, the function and its test).
- Other e2e runs have appended to the same files: keep their additions and resolve conflicts by keeping both sides.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: `pkg/greeting/greeting.go` MUST export a function `HelloE2E202611261652371503() string` that returns exactly the literal `"e2e-cross-repo-spawn-20261126-165237-1503"`.
- **FR-002**: The function MUST have the doc comment `// HelloE2E202611261652371503 returns the run-unique token for the 20261126-165237-1503 cross-repo spawn e2e run.`, following the existing comment convention in the file.
- **FR-003**: `pkg/greeting/greeting_test.go` MUST contain `TestHelloE2E202611261652371503(t *testing.T)`, which declares `const want = "e2e-cross-repo-spawn-20261126-165237-1503"` and fails if `HelloE2E202611261652371503()` returns anything else.
- **FR-004**: The new function and test MUST be appended after the existing ones. Existing functions and tests MUST NOT be reordered, renamed or removed. On a rebase conflict, every other run's functions and tests MUST be kept.
- **FR-005**: The function name and string literal MUST be used exactly as written above, with no alteration, abbreviation or regeneration.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: `HelloE2E202611261652371503` exists on the repository's default branch and returns `"e2e-cross-repo-spawn-20261126-165237-1503"`.
- **SC-002**: `go test ./...` passes in this repository, including the new test.
- **SC-003**: The literal `e2e-cross-repo-spawn-20261126-165237-1503` appears in the default branch after the change is merged.
- **SC-004**: No pre-existing function or test in `pkg/greeting` is renamed, removed or reordered.

## Assumptions

- The module stays at `go 1.22`, and no new dependencies are introduced.
- Only `pkg/greeting/greeting.go` and `pkg/greeting/greeting_test.go` change.
- If `pkg/greeting` does not yet exist, creating the minimum (package `greeting`, the function and its test) is acceptable.

## Out of Scope *(optional)*

- Any change outside `pkg/greeting/greeting.go` and `pkg/greeting/greeting_test.go`.
- Changes to the behavior of any existing function in this repository.
- The cross-repo caller in `handarbeit/fabrik-test-gamma`, which is handled in that repository.

## Source References *(optional)*

- Spawned by Fabrik from parent issue handarbeit/fabrik-test-gamma#2758 as a multi-issue decomposition.
- Scenario background: #803 and #1308, mentioned for context.
