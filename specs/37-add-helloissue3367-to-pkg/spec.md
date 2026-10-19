# Feature Specification: Add HelloE2E202610191221560514 greeting function

**Feature Branch**: `fabrik/issue-37`
**Created**: 2026-10-19
**Status**: Specified
**Input**: User description: "Add one exported function and one test to the `greeting` package of this repo. This is the prerequisite half of an end-to-end cross-repo spawn regression scenario; a downstream repo (`handarbeit/fabrik-test-gamma`) will import this function once it lands. The identifiers below contain a per-run-unique token and must be used **verbatim**. Do not invent, alter or substitute your own names or strings. A final assertion checks this exact text in `pkg/greeting/greeting.go`. In `pkg/greeting/greeting.go`, add an exported function `HelloE2E202610191221560514() string` with a doc comment, matching the layout of the existing `HelloE2E…` functions. It returns exactly the string literal `e2e-cross-repo-spawn-20261019-122156-0514`. In `pkg/greeting/greeting_test.go`, add a test for it, following the existing per-function test shape: `const want = \"e2e-cross-repo-spawn-20261019-122156-0514\"` and `if got := HelloE2E202610191221560514(); got != want { t.Errorf(...) }`. Append the new function and test after the existing ones. Change nothing else: no other edits to the greeting package, no other files, no dependency changes."

## Background

This issue is the prerequisite half of an end-to-end regression scenario for cross-repo child spawning. It was spawned by Fabrik from parent issue `handarbeit/fabrik-test-gamma#467` as part of a multi-issue decomposition. The scenario exercises on-demand spawn-target repo initialisation (#803) and per-run-unique fixture tokens (#1308). A downstream repo (`handarbeit/fabrik-test-gamma`) will pin a commit of this repo that contains the new function, so the change must reach the default branch (`main`). The identifier and string literal embed a per-run-unique token, and a final assertion checks that exact text in `pkg/greeting/greeting.go`, so they must be used verbatim.

The `greeting` package already exists at `pkg/greeting/` and contains a series of sentinel functions of the same shape (`HelloE2E…`), each with a doc comment and a matching per-function test.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Downstream repo can pin a commit containing the new sentinel (Priority: P1)

A maintainer of the downstream e2e scenario needs `HelloE2E202610191221560514` to exist on this repo's default branch so `fabrik-test-gamma` can import it from a pinned commit.

**Why this priority**: It is the only deliverable; the downstream half of the scenario is blocked until it lands.

**Independent Test**: Run `go test ./...` in this repo and grep `pkg/greeting/greeting.go` for the function name and literal.

**Acceptance Scenarios**:

1. **Given** the repo at the merged commit, **When** `HelloE2E202610191221560514()` is called, **Then** it returns exactly `e2e-cross-repo-spawn-20261019-122156-0514`.
2. **Given** the merged commit, **When** `pkg/greeting/greeting.go` is searched, **Then** it contains both `HelloE2E202610191221560514` and `e2e-cross-repo-spawn-20261019-122156-0514`.
3. **Given** the merged commit, **When** `go test ./...` runs, **Then** all tests pass, including the new one.

---

### Edge Cases

- The identifier and literal contain a per-run token; any alteration (typo, different digits) fails the final assertion, so they must be copied exactly.
- Existing `HelloE2E…` functions and tests must remain untouched and in their current order; the new ones are appended after them.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: `pkg/greeting/greeting.go` MUST define an exported function `HelloE2E202610191221560514() string` with a doc comment, in the same layout as the existing `HelloE2E…` functions, appended after the existing ones.
- **FR-002**: `HelloE2E202610191221560514()` MUST return exactly the string `e2e-cross-repo-spawn-20261019-122156-0514`.
- **FR-003**: `pkg/greeting/greeting_test.go` MUST contain a test for the function in the existing per-function shape (`const want = "e2e-cross-repo-spawn-20261019-122156-0514"`, compare `got` to `want`, `t.Errorf` on mismatch), appended after the existing tests.
- **FR-004**: No other changes are made: no other edits to the `greeting` package, no other source files, and no dependency changes.
- **FR-005**: The PR for this issue MUST be merged to the default branch (`main`) so downstream repos can pin a commit that contains the function.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: `pkg/greeting/greeting.go` on `main` contains both `HelloE2E202610191221560514` and `e2e-cross-repo-spawn-20261019-122156-0514`.
- **SC-002**: `go test ./...` passes on the merged commit, including the new test.
- **SC-003**: The diff of the change touches only the greeting source and test files (plus the engine-managed spec file).

## Assumptions

- The `greeting` package files already exist (they do), so no new package needs creating.
- The doc comment follows the existing wording: "returns the sentinel string for the cross-repo spawn regression test."
- The engine-managed `specs/37-*/spec.md` file is produced by the pipeline and is not counted as an out-of-scope change.

## Out of Scope *(optional)*

- Any changes to existing greeting functions or tests.
- Changes to the downstream `handarbeit/fabrik-test-gamma` repo.
- Dependency, build or CI changes.
