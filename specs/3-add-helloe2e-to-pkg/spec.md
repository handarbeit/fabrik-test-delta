# Feature Specification: Add HelloE2E202610142252572433 sentinel to pkg/greeting

**Feature Branch**: `fabrik/issue-3`
**Created**: 2026-10-14
**Status**: Specified
**Input**: User description:

> ## Problem
> `handarbeit/fabrik-test-gamma` (the "alpha" repo) needs to call a new greeting function from this repo's `pkg/greeting` package. That function doesn't exist yet. This work must land and merge first, because gamma cannot import a function that does not yet exist. It is the delta half of a cross-repo e2e scenario, parent issue `handarbeit/fabrik-test-gamma#43`.
>
> ## Requirements
> - In `pkg/greeting/greeting.go`, add an exported function `HelloE2E202610142252572433() string` that returns exactly the literal string `"e2e-cross-repo-spawn-20261014-225257-2433"`.
> - In `pkg/greeting/greeting_test.go`, add a test that calls `HelloE2E202610142252572433()` and fails unless it returns exactly `"e2e-cross-repo-spawn-20261014-225257-2433"`. Follow the existing test style in that file (table-driven if the neighbouring tests are).
> - Use the function name and the string literal verbatim, character for character. Do not invent a different name or value. A per-run-unique token (`2252572433`) is already baked into both, and a downstream assertion checks that this exact text lands in this repo.
> - Follow the pattern of the existing `HelloE2E<token>()` functions in the same file: one exported function per run, with a matching test case.
> - Append the new function and test; do not reorder, rename or remove the existing ones.
>
> ## Scope
> - Only `pkg/greeting/greeting.go` and `pkg/greeting/greeting_test.go` in this repo.
> - No changes to other packages, CI, or docs.
> - No changes in `handarbeit/fabrik-test-gamma`. The parent issue handles the gamma side after this merges.
>
> ## Acceptance
> - `go build ./...` and `go test ./...` pass in this repo.
> - On this repo's default branch after merge, `pkg/greeting/greeting.go` contains `HelloE2E202610142252572433` returning `"e2e-cross-repo-spawn-20261014-225257-2433"`, and `pkg/greeting/greeting_test.go` contains a test for it.
> - The PR carries `Closes #<this issue>` in its body.
>
> *Spawned by Fabrik from parent issue handarbeit/fabrik-test-gamma#43 as a multi-issue decomposition. The parent's plan is at the link above.*

## Background

`handarbeit/fabrik-test-gamma` (the "alpha" repo) needs to call a new greeting function from this repo's `pkg/greeting` package, and that function does not exist yet. Because gamma cannot import something that isn't there, this change must merge first. It is the delta half of a cross-repo e2e scenario whose parent is `handarbeit/fabrik-test-gamma#43`, spawned as part of a multi-issue decomposition.

The package already contains one sentinel function per earlier e2e run (`HelloE2E<token>()`), each returning a fixed string and each with a matching test. This issue adds the next one in that series. The function name and its return value carry a per-run-unique token (`2252572433`), and a downstream assertion checks that this exact text lands in this repo, so the name and value must be reproduced exactly.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Sentinel function available for the gamma repo to import (Priority: P1)

A consumer in `handarbeit/fabrik-test-gamma` needs an exported function in `pkg/greeting` that returns the run's sentinel string, so the cross-repo e2e scenario can proceed once this change has merged.

**Why this priority**: It is the only deliverable of this issue; the gamma-side work is blocked until it lands.

**Independent Test**: Run `go test ./...` in this repo and confirm the new test passes. Then inspect the default branch after merge and confirm both the function and its test are present.

**Acceptance Scenarios**:

1. **Given** the `pkg/greeting` package, **When** `HelloE2E202610142252572433()` is called, **Then** it returns exactly `"e2e-cross-repo-spawn-20261014-225257-2433"`.
2. **Given** the new test, **When** the function returns any value other than that literal, **Then** the test fails.
3. **Given** the change is merged, **When** the default branch is inspected, **Then** `pkg/greeting/greeting.go` contains `HelloE2E202610142252572433` and `pkg/greeting/greeting_test.go` contains a test for it.

---

### Edge Cases

- A typo or alternative spelling in the function name or string, or a different token, would fail the downstream assertion even though this repo's own tests would still pass. Both must match character for character.
- Existing functions and tests must remain untouched and in their original order; the new ones are appended.
- The new test must follow the style of the neighbouring tests in the file so the file stays consistent.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: `pkg/greeting` MUST export a function `HelloE2E202610142252572433() string` that returns exactly the literal `"e2e-cross-repo-spawn-20261014-225257-2433"`.
- **FR-002**: The test file for `pkg/greeting` MUST contain a test that calls `HelloE2E202610142252572433()` and fails unless the result equals `"e2e-cross-repo-spawn-20261014-225257-2433"`.
- **FR-003**: The function name and string literal MUST be used verbatim, with no variation in name, token or value.
- **FR-004**: The new function and test MUST follow the existing `HelloE2E<token>()` pattern in the same files (one exported function per run, one matching test) and MUST be appended after the existing ones, without reordering, renaming or removing any existing function or test.
- **FR-005**: The change MUST be confined to `pkg/greeting/greeting.go` and `pkg/greeting/greeting_test.go`.
- **FR-006**: The pull request body MUST include `Closes #3`.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: `go build ./...` and `go test ./...` pass in this repo with the change applied.
- **SC-002**: After merge, the default branch's `pkg/greeting/greeting.go` contains `HelloE2E202610142252572433` returning `"e2e-cross-repo-spawn-20261014-225257-2433"`, and `pkg/greeting/greeting_test.go` contains a test for it.
- **SC-003**: The diff touches only the two files named in FR-005, and contains only additions to the existing function and test lists.

## Assumptions

- The existing `HelloE2E<token>()` functions and their tests show the current conventions (a doc comment on each function, and a per-function test using a `want` constant). The new code follows them. The existing tests are not table-driven for this series, so a single-case test per function matches the "neighbouring tests" style.
- The new function's doc comment follows the same wording as the other recent `HelloE2E<token>()` functions.

## Out of Scope

- Any change to other packages, CI configuration or documentation.
- Any change in `handarbeit/fabrik-test-gamma`; the parent issue handles the gamma side after this merges.
- Refactoring, reordering or consolidating the existing sentinel functions and tests.
