# Feature Specification: Add HelloE2E202611262151585332 greeting function (e2e cross-repo spawn 20261126-215158-5332)

**Feature Branch**: `fabrik/issue-244`
**Created**: 2026-11-26
**Status**: Specified
**Input**: User description: "Add a per-run-unique e2e fixture function to the `pkg/greeting` package, along with a test for it. A downstream repo (`handarbeit/fabrik-test-gamma`) will import and call this function after this change lands, so it must exist on this repo's default branch.

Requirements:
1. In `pkg/greeting/greeting.go`, add an exported function with this exact name and signature: `func HelloE2E202611262151585332() string`. It must return exactly this literal string, with no changes: `e2e-cross-repo-spawn-20261126-215158-5332`. Follow the existing pattern for earlier e2e functions in the same file. Add a doc comment of the form `// HelloE2E202611262151585332 returns the run-unique token for the 20261126-215158 cross-repo spawn e2e run.`, then the function body `return "e2e-cross-repo-spawn-20261126-215158-5332"`. Append it after the most recent earlier `HelloE2E…` function.
2. In `pkg/greeting/greeting_test.go`, add a test named `TestHelloE2E202611262151585332`. Follow the existing tests for earlier e2e functions: declare `const want = "e2e-cross-repo-spawn-20261126-215158-5332"`, call `HelloE2E202611262151585332()`, and call `t.Errorf("HelloE2E202611262151585332() = %q, want %q", got, want)` when `got != want`.
3. Use the function name and the string literal verbatim. Do not invent, shorten or alter either. A per-run-unique token is already embedded in both, and an end-to-end check depends on this exact text appearing in this repo.

Scope: Only the single function and its single test. No other greeting behaviour. Do not refactor existing code. If the repo keeps a `specs/<N>-…` directory per run, follow the usual convention for this issue.

Acceptance: `pkg/greeting/greeting.go` contains `HelloE2E202611262151585332()` returning `"e2e-cross-repo-spawn-20261126-215158-5332"`. `pkg/greeting/greeting_test.go` contains `TestHelloE2E202611262151585332`. `go build ./...` and `go test ./...` pass in this repo. The change is merged to the default branch."

## Background

This issue was spawned by Fabrik from parent issue `handarbeit/fabrik-test-gamma#2933` as part of a multi-issue decomposition in an end-to-end cross-repo spawn test run (`20261126-215158-5332`). The downstream repo `handarbeit/fabrik-test-gamma` will import and call a run-unique function from this repo's `pkg/greeting` package once this change lands. The function therefore has to exist on this repo's default branch (`main`).

The end-to-end check depends on the exact function name and string literal appearing in this repo. Both embed the per-run-unique token, so neither may be altered, shortened or approximated. Earlier e2e runs left similar `HelloE2E…` functions in the same package, and this change follows that established pattern.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Downstream repo can call the run-unique greeting function (Priority: P1)

A maintainer of the downstream repo `handarbeit/fabrik-test-gamma` needs to import `pkg/greeting` from this repo and call `HelloE2E202611262151585332()`. The call must return the run-unique token so the cross-repo spawn e2e check can confirm the dependency chain works end to end.

**Why this priority**: It is the only deliverable of this issue. The downstream work is blocked until the function exists on `main`.

**Independent Test**: Run `go test ./pkg/greeting/...` in this repo. `TestHelloE2E202611262151585332` passes, and `HelloE2E202611262151585332()` returns `e2e-cross-repo-spawn-20261126-215158-5332`.

**Acceptance Scenarios**:

1. **Given** the change is merged to `main`, **When** a caller invokes `HelloE2E202611262151585332()`, **Then** it returns exactly `e2e-cross-repo-spawn-20261126-215158-5332`.
2. **Given** the change is applied, **When** `go build ./...` and `go test ./...` are run, **Then** both succeed with no failures.
3. **Given** the change is applied, **When** `pkg/greeting/greeting_test.go` is inspected, **Then** it contains `TestHelloE2E202611262151585332`, which compares the function result to the expected literal and reports a mismatch with the message format `HelloE2E202611262151585332() = %q, want %q`.

---

### Edge Cases

- A single-character deviation in the function name or the returned literal (including the timestamp or the `5332` suffix) breaks the downstream e2e check, so both must match the spec exactly.
- The new function must not collide with or alter any earlier `HelloE2E…` function. It is added after the most recent one.
- If this repo keeps a per-issue `specs/<N>-…` directory, the spec file for this issue follows that convention. The engine writes it, not the implementer.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: `pkg/greeting/greeting.go` MUST define an exported function `HelloE2E202611262151585332` with signature `func HelloE2E202611262151585332() string`.
- **FR-002**: `HelloE2E202611262151585332()` MUST return exactly the literal `e2e-cross-repo-spawn-20261126-215158-5332`.
- **FR-003**: The function MUST carry the doc comment `// HelloE2E202611262151585332 returns the run-unique token for the 20261126-215158 cross-repo spawn e2e run.` and be placed after the most recent earlier `HelloE2E…` function, following the existing pattern in the file.
- **FR-004**: `pkg/greeting/greeting_test.go` MUST define `TestHelloE2E202611262151585332`, which declares `const want = "e2e-cross-repo-spawn-20261126-215158-5332"`, calls `HelloE2E202611262151585332()`, and calls `t.Errorf("HelloE2E202611262151585332() = %q, want %q", got, want)` when `got != want`, following the existing tests for earlier e2e functions.
- **FR-005**: The function name and the string literal MUST be used verbatim, with no invented, shortened or altered variants.
- **FR-006**: The change MUST add only this function and its test. No other greeting behaviour is added, and no existing code is refactored.
- **FR-007**: `go build ./...` and `go test ./...` MUST pass in this repo after the change.
- **FR-008**: The change MUST be merged to the default branch (`main`).

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: On `main`, `HelloE2E202611262151585332()` exists and returns `e2e-cross-repo-spawn-20261126-215158-5332`.
- **SC-002**: `TestHelloE2E202611262151585332` exists and passes.
- **SC-003**: `go build ./...` and `go test ./...` complete successfully with zero failures.
- **SC-004**: The downstream repo `handarbeit/fabrik-test-gamma` can import and call the function once the change has merged.
- **SC-005**: The diff touches only the greeting source file, its test file, and the issue's spec file if this repo's convention keeps one.

## Assumptions

- An earlier `HelloE2E…` function and a matching test already exist in `pkg/greeting`, and the new function and test should mirror them. The issue states this explicitly.
- The `specs/<N>-…` directory convention, if present in this repo, is handled by the engine's spec persistence and needs no manual work.
- The issue's provided function name, literal and doc comment text are authoritative and are not to be re-derived.

## Out of Scope

- Any other greeting behaviour or new functions beyond the single function and test.
- Refactoring, renaming or tidying existing `HelloE2E…` functions or tests.
- Changes in `handarbeit/fabrik-test-gamma`, which is handled by its own issue.
- Documentation or CI changes.

## Source References

- Parent issue: `handarbeit/fabrik-test-gamma#2933`
- Existing `HelloE2E…` functions and tests in `pkg/greeting/greeting.go` and `pkg/greeting/greeting_test.go`, as the pattern to follow
