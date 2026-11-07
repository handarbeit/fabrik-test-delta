# Feature Specification: Add HelloE2E202611071936352658 greeting function and test

**Feature Branch**: `fabrik/issue-168`
**Created**: 2026-11-07
**Status**: Specified
**Input**: User description: "## Problem

The gamma test bed (`handarbeit/fabrik-test-gamma`) needs a new exported greeting function from this repo for an end-to-end cross-repo spawn regression scenario. It cannot import the function until it exists here.

## Requirements

- In `pkg/greeting/greeting.go`, add an exported function `HelloE2E202611071936352658() string` that returns exactly the literal `\"e2e-cross-repo-spawn-20261107-193635-2658\"`.
- Follow the existing style of the file: a one-line function with a doc comment, like the other per-run `HelloE2E…` functions. Do not modify or refactor any existing function.
- In `pkg/greeting/greeting_test.go`, add a test for the new function that asserts it returns exactly `\"e2e-cross-repo-spawn-20261107-193635-2658\"`. Follow the style of the existing tests in that file.
- Use the function name and string literal exactly as written above. Do not invent a different name or token. The token `20261107-193635-2658` is fixed for this run and must not be regenerated or altered, because the scenario's final assertion depends on this exact text landing in this repo's default branch.

## Scope

Only the function and its test. No other changes.

## Acceptance

- `pkg/greeting/greeting.go` exports `HelloE2E202611071936352658() string` returning exactly `\"e2e-cross-repo-spawn-20261107-193635-2658\"`.
- `pkg/greeting/greeting_test.go` contains a passing test for it.
- `go build ./...` and `go test ./...` pass.
- The change is merged to the default branch.

*Spawned by Fabrik from parent issue handarbeit/fabrik-test-gamma#1739 as a multi-issue decomposition. The parent's plan is at the link above.*"

## Background

The gamma test bed (`handarbeit/fabrik-test-gamma`) runs an end-to-end cross-repo spawn regression scenario that depends on a new exported greeting function living in this repo. Gamma cannot import that function until it exists here. This repo already carries a series of per-run `HelloE2E…` sentinel functions in the greeting package, each with its own test, added the same way for earlier runs of this scenario. This issue adds the one for the current run.

The token `20261107-193635-2658` is fixed for this run. The scenario's final assertion checks that this exact text lands on this repo's default branch (`main`), so it must not be regenerated or altered.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Gamma can consume the new sentinel function (Priority: P1)

The cross-repo spawn scenario in the gamma test bed needs to import and call `HelloE2E202611071936352658` from this repo's greeting package and see the run's fixed sentinel string.

**Why this priority**: It is the whole purpose of the issue. Without it the gamma scenario cannot complete.

**Independent Test**: Run this repo's build and tests. The new function exists, returns the exact literal, and its test passes.

**Acceptance Scenarios**:

1. **Given** the greeting package on the default branch, **When** a caller invokes `HelloE2E202611071936352658()`, **Then** it returns exactly `"e2e-cross-repo-spawn-20261107-193635-2658"`.
2. **Given** the change is applied, **When** `go build ./...` and `go test ./...` run, **Then** both pass, including the new test.
3. **Given** the change is merged, **When** the default branch is inspected, **Then** the new function and its literal are present.

---

### Edge Cases

- The function name or literal must not differ by even one character from the values given. A near-miss would break the gamma scenario's final assertion.
- Existing `HelloE2E…` functions and their tests must remain unchanged in behavior and text.
- The date-time token must not be regenerated, even though a newer one could be produced.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: `pkg/greeting/greeting.go` MUST export `HelloE2E202611071936352658() string` that returns exactly `"e2e-cross-repo-spawn-20261107-193635-2658"`.
- **FR-002**: The new function MUST follow the file's existing style for per-run `HelloE2E…` functions: a doc comment followed by a one-line body.
- **FR-003**: `pkg/greeting/greeting_test.go` MUST contain a test for the new function that asserts it returns exactly `"e2e-cross-repo-spawn-20261107-193635-2658"`, in the style of the existing tests in that file.
- **FR-004**: No existing function or test may be modified, renamed, reordered or refactored.
- **FR-005**: The function name and string literal MUST be used exactly as written in FR-001, with no substitute name or token.
- **FR-006**: The change MUST contain only the new function and its test, with no other changes.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: `go build ./...` and `go test ./...` both pass with the change applied.
- **SC-002**: The new test passes and fails if the returned literal is altered in any way.
- **SC-003**: After merge, the exact text `e2e-cross-repo-spawn-20261107-193635-2658` is present on the default branch of this repo, so the gamma scenario's final assertion can succeed.
- **SC-004**: The diff touches only the greeting source file and its test file, and no existing lines are changed.

## Assumptions

- The default branch is `main`, as stated in the issue context.
- "Style of the file" means the pattern of the existing per-run `HelloE2E…` functions, each with a one-line doc comment and a one-line `return` of a string literal. The new function is appended after the existing ones.
- The new test follows the existing pattern of a `const want` plus a `got != want` check, appended after the existing tests.
- No documentation or other file needs updating. The issue limits scope to the function and its test.

## Out of Scope *(optional)*

- Modifying, refactoring or removing any existing greeting function or test.
- Any change in the gamma test bed (`handarbeit/fabrik-test-gamma`), including importing or using the new function there.
- Documentation, README or changelog updates.
- Generating or changing the run token.
