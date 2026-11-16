# Feature Specification: Add run-unique e2e greeting function

**Feature Branch**: `fabrik/issue-186`
**Created**: 2026-11-16
**Status**: Specified
**Input**: User description: "# Feature: Add run-unique e2e greeting function

## Problem

The `pkg/greeting` package in this repo needs a new exported function with a run-unique name. A downstream consumer, `handarbeit/fabrik-test-gamma`, imports `github.com/handarbeit/fabrik-test-delta/pkg/greeting` and will call it. The consumer cannot compile until this function exists on this repo's default branch, so this change must merge first.

This is an end-to-end regression scenario for on-demand spawn-target repo initialisation (#803) and per-run-unique fixture tokens (#1308). The final assertion checks for the exact function name and literal below in this repo, so use them verbatim and do not invent your own.

## Requirements

- **FR-001**: Add an exported function `HelloE2E202611161354261489() string` to `pkg/greeting/greeting.go`.
- **FR-002**: It must return exactly the string literal `e2e-cross-repo-spawn-20261116-135426-1489`.
- **FR-003**: Add a test in `pkg/greeting/greeting_test.go` that calls `HelloE2E202611161354261489()` and asserts it returns exactly `e2e-cross-repo-spawn-20261116-135426-1489`.
- **FR-004**: Use the function name and literal verbatim, with no substitution of other tokens.

If `pkg/greeting/greeting.go` or `greeting_test.go` do not exist yet, create them, following the conventions of the existing `HelloE2E<timestamp>` functions and their tests. Do not change or remove any existing function.

## Acceptance Criteria

1. Calling `HelloE2E202611161354261489()` returns `e2e-cross-repo-spawn-20261116-135426-1489`.
2. `go test ./...` passes in this repo, including the new test.
3. The exact text `HelloE2E202611161354261489` and `e2e-cross-repo-spawn-20261116-135426-1489` is present in this repo's default branch after merge.

## Out of Scope

- Any greeting behaviour beyond this single function and its test.
- Changes to other packages or files.
- Any change in `handarbeit/fabrik-test-gamma`; that repo's follow-up change is tracked separately and depends on this one."

## Background

The `pkg/greeting` package in this repo needs a new exported function with a run-unique name. A downstream consumer, `handarbeit/fabrik-test-gamma`, imports `github.com/handarbeit/fabrik-test-delta/pkg/greeting` and will call it. The consumer cannot compile until this function exists on this repo's default branch (`main`), so this change must merge first.

This is an end-to-end regression scenario for on-demand spawn-target repo initialisation (#803) and per-run-unique fixture tokens (#1308). The final assertion checks for the exact function name and literal below in this repo, so they must be used verbatim.

The package already holds earlier run-unique sentinel functions in the same `HelloE2E<timestamp>` form, each returning a matching `e2e-cross-repo-spawn-<timestamp>` string and each covered by a test. The new function follows that established pattern.

This issue was spawned by Fabrik from parent issue `handarbeit/fabrik-test-gamma#1989` as part of a multi-issue decomposition.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Downstream consumer can call the run-unique greeting (Priority: P1)

The consumer repo `handarbeit/fabrik-test-gamma` imports the greeting package and calls `HelloE2E202611161354261489()`. Once this change is on `main`, the consumer compiles and the function returns the expected sentinel string.

**Why this priority**: It is the entire purpose of the issue. The consumer cannot compile, and the e2e scenario cannot pass, until it exists.

**Independent Test**: Run this repo's tests. The new test calls `HelloE2E202611161354261489()` and asserts the exact return value.

**Acceptance Scenarios**:

1. **Given** the greeting package on `main` after merge, **When** `HelloE2E202611161354261489()` is called, **Then** it returns `e2e-cross-repo-spawn-20261116-135426-1489`.
2. **Given** the change is applied, **When** `go test ./...` is run in this repo, **Then** all tests pass, including the new one.
3. **Given** the change has merged, **When** the e2e final assertion searches this repo's default branch, **Then** both the exact text `HelloE2E202611161354261489` and `e2e-cross-repo-spawn-20261116-135426-1489` are found.

---

### Edge Cases

- The function name and literal contain run-unique digits; any transposition or substitution breaks the downstream consumer and the final assertion, so both are used exactly as given.
- Existing `HelloE2E<timestamp>` functions and their tests must remain untouched.
- If the greeting source or test file were missing, they would be created following the conventions of the existing sentinel functions. At present both files exist.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: The greeting package exports a function `HelloE2E202611161354261489() string`.
- **FR-002**: `HelloE2E202611161354261489()` returns exactly the string `e2e-cross-repo-spawn-20261116-135426-1489`.
- **FR-003**: The greeting package's tests include a test that calls `HelloE2E202611161354261489()` and asserts it returns exactly `e2e-cross-repo-spawn-20261116-135426-1489`.
- **FR-004**: The function name and the string literal are used verbatim, with no substitution of other tokens.
- **FR-005**: No existing function in the greeting package, and no existing test, is changed or removed.

### Key Entities *(if applicable)*

- **Run-unique sentinel function**: An exported greeting-package function whose name embeds a per-run timestamp token and which returns a matching literal string, used by the e2e scenario to prove cross-repo spawn and consumption.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: Calling `HelloE2E202611161354261489()` returns `e2e-cross-repo-spawn-20261116-135426-1489` exactly.
- **SC-002**: The repository's full test run passes, including the new test.
- **SC-003**: After merge, the exact text `HelloE2E202611161354261489` and `e2e-cross-repo-spawn-20261116-135426-1489` is present on the default branch.
- **SC-004**: The downstream consumer repo can compile against the merged default branch.

## Assumptions

- The existing `HelloE2E<timestamp>` functions and their tests define the conventions to follow (naming, doc comment, test style).
- The default branch is `main`.
- No other behaviour is expected of this function beyond returning the fixed string.

## Out of Scope *(optional)*

- Any greeting behaviour beyond this single function and its test.
- Changes to other packages or files.
- Any change in `handarbeit/fabrik-test-gamma`; that repo's follow-up change is tracked separately and depends on this one.
