# Feature Specification: Add HelloE2E202610150720020653 greeting function

**Feature Branch**: `fabrik/issue-9`
**Created**: 2026-10-15
**Status**: Specified
**Input**: User description: "Add one exported function and one test to the `pkg/greeting` package in this repo. A downstream consumer, `handarbeit/fabrik-test-gamma`, will call the function once this change has merged. This is a per-run fixture for an end-to-end cross-repo spawn regression test, so the function name and string literal below must be used exactly as written. Do not rename them, "improve" them or invent a different token."

## Background

This issue is a per-run fixture for an end-to-end cross-repo spawn regression test. It was spawned from parent issue `handarbeit/fabrik-test-gamma#111`. The parent's repository, `handarbeit/fabrik-test-gamma`, will consume a new greeting function from this repository once this change has merged. The test only works if the function name and returned token match the values the consumer expects. The values are therefore fixed and must not be altered or improved. The existing `HelloE2E…` functions in the same package are earlier fixtures of the same kind, and the new one follows their pattern.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Greeting function and its test exist in `pkg/greeting` (Priority: P1)

A downstream consumer (`handarbeit/fabrik-test-gamma`) needs an exported greeting function that returns a known, exact token. Once this change has merged, the consumer can call the function and compare the result to that token.

**Why this priority**: It is the only deliverable. The cross-repo spawn regression test depends on it.

**Independent Test**: Run `go test ./...` in this repository. `TestHelloE2E202610150720020653` exists and passes. Calling `HelloE2E202610150720020653()` returns exactly `e2e-cross-repo-spawn-20261015-072002-0653`.

**Acceptance Scenarios**:

1. **Given** the greeting package, **When** `HelloE2E202610150720020653()` is called, **Then** it returns exactly `e2e-cross-repo-spawn-20261015-072002-0653`.
2. **Given** the repo's test suite, **When** `go test ./...` is run, **Then** `TestHelloE2E202610150720020653` exists and passes.

---

### Edge Cases

- If `pkg/greeting/greeting.go` or its test file does not exist yet, they are created with the minimum content needed for the function and test.
- The function name and the string literal are copied exactly. A similar-looking name or token (a different timestamp digit, a hyphen moved) would silently break the downstream consumer.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: In `pkg/greeting/greeting.go`, add an exported function named exactly `HelloE2E202610150720020653`. It takes no arguments and returns a `string`.
- **FR-002**: The function MUST return exactly the literal `e2e-cross-repo-spawn-20261015-072002-0653`.
- **FR-003**: In `pkg/greeting/greeting_test.go`, add a test named `TestHelloE2E202610150720020653`. It asserts that the function returns exactly `e2e-cross-repo-spawn-20261015-072002-0653`.
- **FR-004**: Follow the style of the existing `HelloE2E…` functions and tests in the same files. Each has a single doc comment, one function, and one test using a `const want` and a `got != want` check. Append the new function and test, and do not modify any existing function or test.
- **FR-005**: `go build ./...` and `go test ./...` must pass.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: After merge, a consumer calling `HelloE2E202610150720020653()` receives exactly `e2e-cross-repo-spawn-20261015-072002-0653`, with no variation.
- **SC-002**: `go build ./...` and `go test ./...` both succeed on the merged change, and the new test is among the passing tests.
- **SC-003**: The diff only adds the new function and its test (plus the engine's spec file). No existing function or test is modified.

## Assumptions

- The existing `HelloE2E…` functions and tests in `pkg/greeting` define the style to follow. If the files do not exist, the minimum content needed is acceptable.
- The function name and string literal are fixed by the downstream consumer and take precedence over any stylistic preference.

## Out of Scope *(optional)*

- Any change to other functions in the greeting package.
- Any additional functions, tokens or files.
- Any change to engine, CI or build configuration.

## Goal

Add one exported function and one test to the `pkg/greeting` package in this repo. A downstream consumer, `handarbeit/fabrik-test-gamma`, will call the function once this change has merged. This is a per-run fixture for an end-to-end cross-repo spawn regression test, so the function name and string literal below must be used exactly as written. Do not rename them, "improve" them or invent a different token.

## Notes

If `pkg/greeting/greeting.go` or its test file does not exist yet, create them with the minimum content needed for the function and test above.
---

*Spawned by Fabrik from parent issue handarbeit/fabrik-test-gamma#111 as a multi-issue decomposition. The parent's plan is at the link above.*
