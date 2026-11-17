# Feature Specification: Add HelloE2E202611170956090573 to pkg/greeting

**Feature Branch**: `fabrik/issue-208`
**Created**: 2026-11-17
**Status**: Specified
**Input**: User description: "## Goal

Add one new exported greeting function and its test to `pkg/greeting`. A downstream repo (`handarbeit/fabrik-test-gamma`) will import this function after this change has merged to `main`, so this change must land first.

## Required change

1. In `pkg/greeting/greeting.go`, add an exported function with exactly this name and body, following the same one-line-plus-doc-comment shape as the existing `HelloE2E…` functions:

   ```go
   // HelloE2E202611170956090573 returns the run-unique token for the e2e cross-repo spawn scenario.
   func HelloE2E202611170956090573() string { return \"e2e-cross-repo-spawn-20261117-095609-0573\" }
   ```

2. In `pkg/greeting/greeting_test.go`, add `TestHelloE2E202611170956090573`. It follows the shape of the existing `TestHelloE2E…` tests: `const want = \"e2e-cross-repo-spawn-20261117-095609-0573\"`, then `if got := HelloE2E202611170956090573(); got != want { t.Errorf(...) }`.

## Constraints

- Use the function name `HelloE2E202611170956090573` and the string literal `e2e-cross-repo-spawn-20261117-095609-0573` exactly as written above. Do not alter, reformat or invent variants. A per-run-unique token is baked into both, and a downstream assertion depends on this exact text landing on this repo's default branch.
- Only add the one function and its one test. Do not modify, remove or reorder any other function in the package. Do not change `go.mod`.
- If `pkg/greeting/greeting.go` or `greeting_test.go` do not exist, create them with package name `greeting`.

## Acceptance criteria

- `pkg/greeting/greeting.go` on the default branch contains an exported `HelloE2E202611170956090573() string` returning exactly `e2e-cross-repo-spawn-20261117-095609-0573`.
- `pkg/greeting/greeting_test.go` contains a passing test for it.
- `go build ./...` and `go test ./...` pass.
- The PR body includes `Closes #<this issue>`."

## Background

This issue is a child spawned from parent issue `handarbeit/fabrik-test-gamma#2435` as part of a cross-repo decomposition in an end-to-end scenario. A downstream repository (`handarbeit/fabrik-test-gamma`) will import a new greeting function from this repository's `pkg/greeting` package, so the function must exist on this repository's default branch (`main`) before the downstream work can proceed. The function and its return value carry a per-run-unique token that a downstream assertion checks, so the exact text matters.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Expose the run-unique greeting token (Priority: P1)

A downstream repository imports `pkg/greeting` from this repository and calls `HelloE2E202611170956090573()` to obtain the run-unique token for the cross-repo spawn scenario.

**Why this priority**: This is the whole change; the downstream repo is blocked until it lands on `main`.

**Independent Test**: Run `go test ./pkg/greeting/...` and confirm `TestHelloE2E202611170956090573` passes; confirm the package builds with `go build ./...`.

**Acceptance Scenarios**:

1. **Given** the change is merged to `main`, **When** a caller invokes `HelloE2E202611170956090573()`, **Then** it returns exactly `e2e-cross-repo-spawn-20261117-095609-0573`.
2. **Given** the change is applied, **When** `go build ./...` and `go test ./...` run, **Then** both succeed.
3. **Given** the change is applied, **When** the existing `HelloE2E…` functions and tests are compared with their prior state, **Then** none were modified, removed or reordered.

---

### Edge Cases

- `pkg/greeting/greeting.go` or `greeting_test.go` is missing: it is created with package name `greeting` (both currently exist, so this should not occur).
- Any difference in the function name or token string (case, whitespace, digits) would break the downstream assertion, so no variants are permitted.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: `pkg/greeting` MUST export a function `HelloE2E202611170956090573() string` that returns exactly `e2e-cross-repo-spawn-20261117-095609-0573`.
- **FR-002**: The function MUST carry the doc comment `HelloE2E202611170956090573 returns the run-unique token for the e2e cross-repo spawn scenario.` and follow the same one-function-plus-doc-comment shape as the existing `HelloE2E…` functions.
- **FR-003**: `pkg/greeting` MUST include a test `TestHelloE2E202611170956090573` that asserts the function's return value equals `e2e-cross-repo-spawn-20261117-095609-0573`, following the shape of the existing `TestHelloE2E…` tests.
- **FR-004**: The change MUST add only this one function and this one test; no other function in the package may be modified, removed or reordered, and `go.mod` MUST NOT change.
- **FR-005**: The pull request body MUST include `Closes #208`.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: After merge, `pkg/greeting/greeting.go` on `main` contains `HelloE2E202611170956090573` returning exactly `e2e-cross-repo-spawn-20261117-095609-0573`.
- **SC-002**: `go build ./...` and `go test ./...` pass with the new test included.
- **SC-003**: The downstream repository's assertion on the token succeeds once it imports the function.

## Assumptions

- The package and test file already exist with package name `greeting`, and the existing `HelloE2E…` functions and tests are the style reference.
- The token and function name in the issue are authoritative and used verbatim.
- The change lands on `main` before the downstream repository consumes it.

## Out of Scope

- Changes to any other function or test in `pkg/greeting`.
- Changes to `go.mod` or any other package.
- The downstream work in `handarbeit/fabrik-test-gamma`.
