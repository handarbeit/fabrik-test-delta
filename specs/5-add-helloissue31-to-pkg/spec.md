# Feature Specification: Add HelloE2E202610142327011219 sentinel to pkg/greeting

**Feature Branch**: `fabrik/issue-5`
**Created**: 2026-10-14
**Status**: Specified
**Input**: User description: "## Goal

Add one exported function and a matching test to the `pkg/greeting` package. This is the delta (beta) half of a cross-repo e2e regression scenario. A consumer repo, `handarbeit/fabrik-test-gamma`, will import this function once it has merged, so this work must land first.

## Requirements

- In `pkg/greeting/greeting.go`, append an exported function `HelloE2E202610142327011219() string` that returns exactly the literal string `e2e-cross-repo-spawn-20261014-232701-1219`. Follow the style of the existing `HelloE2E…` functions in that file: a doc comment, then a one-line body.
- In `pkg/greeting/greeting_test.go`, append `TestHelloE2E202610142327011219`, following the existing per-function test style: a `const want` holding the literal, compare `HelloE2E202610142327011219()` against it, and call `t.Errorf` on mismatch.
- Use the function name and the string literal verbatim. A per-run-unique token is already baked into both, and the scenario's final assertion depends on this exact text landing in this repo. Do not invent a different name or value.
- The change must be strictly additive. Do not modify or remove any existing function or test.

## Scope

Only `pkg/greeting/greeting.go` and `pkg/greeting/greeting_test.go`. No `go.mod` changes are expected, since the module has no dependencies.

## Acceptance

- `HelloE2E202610142327011219` is present on this repo's `main` branch and returns `e2e-cross-repo-spawn-20261014-232701-1219`.
- `go build ./...` and `go test ./...` pass.
- The PR body contains `Closes #<this issue number>`.

*Spawned by Fabrik from parent issue handarbeit/fabrik-test-gamma#70 as a multi-issue decomposition. The parent's plan is at the link above.*"

## Background

This issue is the delta half of a cross-repo end-to-end regression scenario for issue spawning. The parent issue, `handarbeit/fabrik-test-gamma#70`, was decomposed into work in more than one repository. A consumer repo, `handarbeit/fabrik-test-gamma`, will import a new sentinel function from this repo's `pkg/greeting` package. The consumer can only do that after the function exists on this repo's `main`, so this work must merge first.

The function name and its return value both embed a per-run-unique token. The scenario's final assertion checks that this exact text landed in this repo, so neither may be altered. The package already holds several sentinel functions of the same kind from earlier runs of the scenario. Each has a doc comment and a one-line body returning a literal, and each has a matching test.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Provide the sentinel function and its test (Priority: P1)

A consumer repo needs `greeting.HelloE2E202610142327011219()` to exist on this repo's `main` and to return the agreed literal. The package gains that function and a test that pins its return value.

**Why this priority**: It is the only deliverable. The consumer repo's work and the scenario's final assertion both depend on it.

**Independent Test**: Run `go build ./...` and `go test ./...` in this repo. The new test passes. Calling the function returns `e2e-cross-repo-spawn-20261014-232701-1219`.

**Acceptance Scenarios**:

1. **Given** the `pkg/greeting` package on `main`, **When** `HelloE2E202610142327011219()` is called, **Then** it returns exactly `e2e-cross-repo-spawn-20261014-232701-1219`.
2. **Given** the new function and test are in place, **When** `go build ./...` and `go test ./...` run, **Then** both succeed.
3. **Given** the change, **When** the diff is reviewed, **Then** it only adds lines to `pkg/greeting/greeting.go` and `pkg/greeting/greeting_test.go`, and no existing function or test is modified or removed.

---

### Edge Cases

- A typo in the function name or the literal would break the scenario's final assertion and the consumer's import, so both must match verbatim.
- The existing sentinel functions and tests must keep their behavior, because the change is strictly additive.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: `pkg/greeting` exposes an exported function `HelloE2E202610142327011219() string` that returns exactly the literal `e2e-cross-repo-spawn-20261014-232701-1219`.
- **FR-002**: The function follows the style of the existing `HelloE2E…` functions: a doc comment, then a one-line body.
- **FR-003**: A test named `TestHelloE2E202610142327011219` exists in the package's test file. It holds the expected literal in a `const want`, compares the function's result against it, and reports a mismatch with `t.Errorf`.
- **FR-004**: The function name and the literal are used verbatim, with no renaming and no alternative value.
- **FR-005**: The change is strictly additive. No existing function or test is modified or removed.
- **FR-006**: The change touches only `pkg/greeting/greeting.go` and `pkg/greeting/greeting_test.go`. It makes no `go.mod` changes.
- **FR-007**: The PR body contains `Closes #5`.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: `HelloE2E202610142327011219` is present on this repo's `main` branch and returns `e2e-cross-repo-spawn-20261014-232701-1219`.
- **SC-002**: `go build ./...` and `go test ./...` both pass, including the new test.
- **SC-003**: The diff against `main` contains only added lines, in exactly the two in-scope files.
- **SC-004**: The consumer repo `handarbeit/fabrik-test-gamma` can import the function after this work merges.

## Assumptions

- The module has no dependencies, so no `go.mod` change is needed.
- The new function and test go at the end of their files, after the most recent existing sentinel (`HelloE2E202610142252572433`).
- The doc comment wording follows the existing sentinels: "returns the sentinel string for the cross-repo spawn regression test."

## Out of Scope

- Any change to the consumer repo `handarbeit/fabrik-test-gamma`.
- Any change to existing sentinel functions or tests, including cleanup of old ones.
- Any change to `go.mod`, docs, or files other than the two named above.
- Any other behavior in `pkg/greeting`.
