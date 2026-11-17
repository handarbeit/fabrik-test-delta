# Feature Specification: Add HelloE2E202611170510559531 greeting function for e2e cross-repo spawn

**Feature Branch**: `fabrik/issue-192`
**Created**: 2026-11-17
**Status**: Specified
**Input**: User description: "Add one exported function and a test to the `greeting` package at `pkg/greeting`. This is the beta half of an e2e cross-repo spawn regression scenario. A consumer repo (`handarbeit/fabrik-test-gamma`) calls it only after this change has landed on the default branch.

Requirements:

1. In `pkg/greeting/greeting.go`, add an exported function with exactly this name and body:

   ```go
   // HelloE2E202611170510559531 returns the run-unique token for the e2e cross-repo spawn scenario.
   func HelloE2E202611170510559531() string {
       return "e2e-cross-repo-spawn-20261117-051055-9531"
   }
   ```

   Use the function name `HelloE2E202611170510559531` and the string literal `e2e-cross-repo-spawn-20261117-051055-9531` verbatim. Do not change a digit, add a prefix or suffix, or invent a different token. The scenario's final assertion checks this exact text in this repo's default branch.

2. In `pkg/greeting/greeting_test.go`, add a test that calls `HelloE2E202611170510559531()` and fails unless the result equals `\"e2e-cross-repo-spawn-20261117-051055-9531\"`.

3. If `pkg/greeting/greeting.go` or `pkg/greeting/greeting_test.go` does not exist yet, create it with the correct `package greeting` declaration, then add the function and test. This covers on-demand repo initialisation (#803).

Acceptance Criteria:

- `HelloE2E202611170510559531()` is exported from `pkg/greeting` and returns exactly `e2e-cross-repo-spawn-20261117-051055-9531`.
- A test for it exists in `pkg/greeting/greeting_test.go`, and `go test ./pkg/greeting/...` passes.
- Nothing else in the package changes. Existing functions and tests are untouched.
- The change is merged to the repository's default branch, so a consumer can pin it.

Out of Scope:

- Any other change to the greeting package.
- Any change to other repos."

## Background

This issue was spawned by Fabrik from parent issue `handarbeit/fabrik-test-gamma#2253` as part of a multi-issue decomposition. It is the "beta" half of an end-to-end regression scenario for cross-repo spawning. The "gamma" half is a consumer repo (`handarbeit/fabrik-test-gamma`), which may call this function only after the change has landed on this repo's default branch (`main`).

The scenario's final assertion checks for the exact function name and token text in this repo's default branch. Both are run-unique (`20261117-051055-9531`), so any deviation, such as a changed digit, a prefix or suffix, or a different token, would make the scenario fail.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Consumer can pin a run-unique greeting token (Priority: P1)

A consumer repo (`handarbeit/fabrik-test-gamma`) needs to call `greeting.HelloE2E202611170510559531()` and receive the run-unique token. The function must exist on this repo's default branch before the consumer pins it.

**Why this priority**: It is the only deliverable of this issue. The cross-repo scenario cannot complete without it.

**Independent Test**: Run `go test ./pkg/greeting/...` in this repo. The new test passes, and the exported function returns exactly `e2e-cross-repo-spawn-20261117-051055-9531`.

**Acceptance Scenarios**:

1. **Given** the change is merged to `main`, **When** a consumer imports `pkg/greeting` and calls `HelloE2E202611170510559531()`, **Then** it returns `e2e-cross-repo-spawn-20261117-051055-9531`.
2. **Given** the change is applied, **When** `go test ./pkg/greeting/...` is run, **Then** all tests pass, including the new one.
3. **Given** the function's return value were altered in any way, **When** the new test runs, **Then** the test fails.

---

### Edge Cases

- `pkg/greeting/greeting.go` does not exist yet (on-demand repo initialisation, #803): create it with `package greeting`, then add the function.
- `pkg/greeting/greeting_test.go` does not exist yet: create it with `package greeting`, then add the test.
- The package already contains other functions and tests: they must remain untouched.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: `pkg/greeting/greeting.go` MUST export a function named exactly `HelloE2E202611170510559531` that takes no arguments and returns a `string`.
- **FR-002**: `HelloE2E202611170510559531()` MUST return exactly the string literal `e2e-cross-repo-spawn-20261117-051055-9531`, with no digit changed and no prefix or suffix added.
- **FR-003**: The function MUST carry the doc comment `// HelloE2E202611170510559531 returns the run-unique token for the e2e cross-repo spawn scenario.`
- **FR-004**: `pkg/greeting/greeting_test.go` MUST contain a test that calls `HelloE2E202611170510559531()` and fails unless the result equals `"e2e-cross-repo-spawn-20261117-051055-9531"`.
- **FR-005**: If `pkg/greeting/greeting.go` or `pkg/greeting/greeting_test.go` does not exist, it MUST be created with the `package greeting` declaration before the function or test is added.
- **FR-006**: No other function, test or file in the `greeting` package may change.
- **FR-007**: The change MUST be merged to the repository's default branch (`main`) so a consumer can pin it.

### Key Entities

- **`greeting` package** (`pkg/greeting`): The Go package that gains the new exported function.
- **Run-unique token**: The string `e2e-cross-repo-spawn-20261117-051055-9531`, which the cross-repo scenario's final assertion checks for verbatim on the default branch.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: `go test ./pkg/greeting/...` passes, with the new test included.
- **SC-002**: The default branch contains the exact function name `HelloE2E202611170510559531` and the exact token `e2e-cross-repo-spawn-20261117-051055-9531`, so the scenario's final assertion succeeds.
- **SC-003**: The diff touches only `pkg/greeting/greeting.go` and `pkg/greeting/greeting_test.go`, and no existing function or test in them is modified.
- **SC-004**: The change is merged to the default branch, so a consumer repo can pin it.

## Assumptions

- The default branch is `main`, as stated by the pipeline context.
- The token and function name are used verbatim from the issue and are never "corrected" or regenerated.
- The Fabrik engine's own spec file under `specs/` is a projection of this body and is not part of the package change this issue describes.

## Out of Scope *(optional)*

- Any other change to the greeting package.
- Any change to other repos, including the consumer repo `handarbeit/fabrik-test-gamma`.
