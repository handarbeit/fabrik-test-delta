# Feature Specification: add HelloE2E202610260819152314 to pkg/greeting

**Feature Branch**: `fabrik/issue-89`
**Created**: 2026-10-26
**Status**: Specified
**Input**: User description: "A consumer repo (`handarbeit/fabrik-test-gamma`) needs to call a new greeting function from this module's `pkg/greeting` package. The function does not exist yet. The consumer cannot compile its change until this repo has it, so this change must land first. FR-001: Add an exported function `HelloE2E202610260819152314() string` to `pkg/greeting/greeting.go`. FR-002: The function MUST return exactly the string literal `e2e-cross-repo-spawn-20261026-081915-2314`. FR-003: Add a test for the function in `pkg/greeting/greeting_test.go`. It MUST assert the exact return value above, and it MUST pass. FR-004: Use the function name and string literal verbatim. Do not rename, paraphrase or regenerate them. They embed a per-run-unique token, and an end-to-end assertion checks for this exact text in this repo. Scope: Only `pkg/greeting/greeting.go` and `pkg/greeting/greeting_test.go` should change. Follow the style of the existing `HelloE2E…` functions and their tests in those files. Read the most recent one first and mirror it. Do not change any other function, file or dependency. Acceptance: `HelloE2E202610260819152314()` returns exactly `e2e-cross-repo-spawn-20261026-081915-2314`. `go build ./...` and `go test ./...` pass in this repo, including the new test. The change is merged to the default branch, so the consumer can pin to a commit that contains it."

## Background

A consumer repository (`handarbeit/fabrik-test-gamma`) needs to call a new greeting function from this module's `pkg/greeting` package. The function does not exist yet, and the consumer cannot compile its change until this repository provides it. This change is therefore a prerequisite and must land first. This is part of an end-to-end cross-repo spawn regression test: the function name and returned string embed a per-run-unique token, and an end-to-end assertion checks for this exact text in this repository. The package already contains a series of structurally identical `HelloE2E…` sentinel functions, each with a matching test; this change adds one more in the same pattern.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Consumer can call the new greeting function (Priority: P1)

The consumer repo (`fabrik-test-gamma`) pins to a commit of this module that contains `HelloE2E202610260819152314` and calls it, receiving the agreed sentinel string.

**Why this priority**: It is the whole purpose of the change; the consumer's work is blocked until it lands.

**Independent Test**: Run the new unit test in the greeting package, then build and test the whole module.

**Acceptance Scenarios**:

1. **Given** the change is merged to the default branch, **When** a caller invokes `HelloE2E202610260819152314()`, **Then** it returns exactly `e2e-cross-repo-spawn-20261026-081915-2314`.
2. **Given** the change is applied, **When** `go build ./...` and `go test ./...` are run, **Then** both succeed, including the new test.
3. **Given** the change is merged, **When** the consumer pins to a commit on the default branch, **Then** that commit contains the new function.

---

### Edge Cases

- The name or literal is altered in any way (typo, re-generated token, different casing): the end-to-end assertion fails, so both must be reproduced character for character.
- Existing `HelloE2E…` functions and tests must remain unchanged and passing.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: Add an exported function `HelloE2E202610260819152314() string` to `pkg/greeting/greeting.go`.
- **FR-002**: The function MUST return exactly the string literal `e2e-cross-repo-spawn-20261026-081915-2314`.
- **FR-003**: Add a test for the function in `pkg/greeting/greeting_test.go` that asserts the exact return value above and passes.
- **FR-004**: The function name and string literal MUST be used verbatim, with no renaming, paraphrasing or regeneration.
- **FR-005**: The new function and test MUST mirror the style of the most recent existing `HelloE2E…` function and test in those files.
- **FR-006**: No other function, file or dependency is changed.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: `HelloE2E202610260819152314()` returns exactly `e2e-cross-repo-spawn-20261026-081915-2314`.
- **SC-002**: `go build ./...` and `go test ./...` pass in this repository, including the new test.
- **SC-003**: The change is merged to the default branch (`main`), so the consumer can pin to a commit containing it.
- **SC-004**: The diff touches only `pkg/greeting/greeting.go` and `pkg/greeting/greeting_test.go`, plus the engine-generated spec file for this issue.

## Assumptions

- The most recent existing `HelloE2E…` function and test are the authoritative style reference and need no additional documentation or doc comments beyond what they have.
- The engine-committed `specs/` file for this issue is a generated artifact and does not count against the "only two files change" scope.

## Out of Scope

- Changes to any other function, file or dependency in this repository.
- Changes in the consumer repo (`handarbeit/fabrik-test-gamma`); that work follows once this lands.
- Removing, renaming or refactoring the existing `HelloE2E…` functions.

## Source References

- Spawned by Fabrik from parent issue `handarbeit/fabrik-test-gamma#1220` as a multi-issue decomposition.
