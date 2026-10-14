# Feature Specification: Add HelloE2E202610141933573941 greeting function and test (e2e cross-repo spawn)

**Feature Branch**: `fabrik/issue-1`
**Created**: 2026-10-14
**Status**: Specified
**Input**: User description: "Add one exported function and a test to the `pkg/greeting` package of this repo (`handarbeit/fabrik-test-delta`). A downstream repo, `handarbeit/fabrik-test-gamma`, will import and call it once this change is merged to the default branch, so this change must land first. In `pkg/greeting/greeting.go`, add the exported function `HelloE2E202610141933573941() string`. It must return exactly the string literal `e2e-cross-repo-spawn-20261014-193357-3941`. In `pkg/greeting/greeting_test.go`, add a test for it. The test must assert that the return value equals `e2e-cross-repo-spawn-20261014-193357-3941`. It must pass with `go test ./...`. Follow the existing style of the other `HelloE2E…` functions and tests in the same files, if there are any. Use the function name `HelloE2E202610141933573941` and the string `e2e-cross-repo-spawn-20261014-193357-3941` exactly as written; do not rename, shorten, regenerate or substitute them. A per-run-unique token is baked into both, and an end-to-end check will search this repo's default branch for that exact text. Do not change any other function, behaviour or file beyond what is needed for the function and its test."

## Background

This issue was spawned by `handarbeit/fabrik-test-gamma#26`, an end-to-end regression scenario for #803 (on-demand spawn-target repo init) and #1308 (per-run-unique fixture token). A downstream repo, `handarbeit/fabrik-test-gamma`, will import and call the new function once it is on this repo's default branch, so this change has to merge first.

The function name and the returned string both contain a per-run-unique token. An end-to-end check searches this repo's default branch for that exact text, so neither may be altered. The `pkg/greeting` package already contains many sibling `HelloE2E…` functions and tests, which set the style to follow.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Downstream repo can call the new greeting function (Priority: P1)

A consumer of `pkg/greeting` (in particular `handarbeit/fabrik-test-gamma`) imports the package and calls `HelloE2E202610141933573941()`. It gets back the agreed sentinel string.

**Why this priority**: This is the whole deliverable. The downstream repo's end-to-end scenario cannot proceed without it.

**Independent Test**: Call `HelloE2E202610141933573941()` and compare the result to `e2e-cross-repo-spawn-20261014-193357-3941`. A unit test in the package does this.

**Acceptance Scenarios**:

1. **Given** the change is merged to the default branch, **When** a caller invokes `HelloE2E202610141933573941()`, **Then** it returns exactly `e2e-cross-repo-spawn-20261014-193357-3941`.
2. **Given** the repo's default branch after merge, **When** the end-to-end check searches it for `HelloE2E202610141933573941` and `e2e-cross-repo-spawn-20261014-193357-3941`, **Then** both strings are found.

---

### User Story 2 - The behaviour is covered by a passing test (Priority: P1)

A maintainer running the package's test suite sees a test that asserts the function's return value, and the suite passes.

**Why this priority**: Required by the issue; it also guards the exact string the downstream check depends on.

**Independent Test**: Run `go test ./...`; the new test passes and asserts equality with the exact string.

**Acceptance Scenarios**:

1. **Given** the new test in `pkg/greeting/greeting_test.go`, **When** `go test ./...` runs, **Then** the test passes.
2. **Given** the function returned a different string, **When** the test runs, **Then** it fails.

---

### Edge Cases

- The name and string must be reproduced character for character; any typo, truncation or regeneration silently breaks the downstream end-to-end search.
- The PR must mention `handarbeit/fabrik-test-gamma#26` by bare number only, with no closing keyword (`Closes`, `Fixes`, `Resolves`) next to it.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: `pkg/greeting/greeting.go` MUST export a function `HelloE2E202610141933573941() string`.
- **FR-002**: `HelloE2E202610141933573941()` MUST return exactly the string `e2e-cross-repo-spawn-20261014-193357-3941`.
- **FR-003**: `pkg/greeting/greeting_test.go` MUST contain a test asserting that `HelloE2E202610141933573941()` equals `e2e-cross-repo-spawn-20261014-193357-3941`.
- **FR-004**: The new function and test MUST follow the style of the existing `HelloE2E…` functions and tests in the same files.
- **FR-005**: No other function, behaviour or file may change beyond what the new function and its test require.
- **FR-006**: The PR MUST mention `handarbeit/fabrik-test-gamma#26` by bare number only, without a closing keyword.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: `HelloE2E202610141933573941()` returns `e2e-cross-repo-spawn-20261014-193357-3941`, verified by an automated test.
- **SC-002**: `go test ./...` passes with the new test included.
- **SC-003**: After the PR merges, both `HelloE2E202610141933573941` and `e2e-cross-repo-spawn-20261014-193357-3941` appear on the repo's default branch.
- **SC-004**: The diff touches only `pkg/greeting/greeting.go` and `pkg/greeting/greeting_test.go`, apart from the spec file the engine commits.

## Assumptions

- The default branch is `main`, and the downstream repo consumes the change only after it is merged there.
- The existing `HelloE2E…` functions and tests show a consistent style that the new additions can mirror.
- The token in the name and string is final and must not be regenerated, even if it looks arbitrary.

## Out of Scope *(optional)*

- Any change to the downstream repo `handarbeit/fabrik-test-gamma`.
- Changes to existing greeting functions, tests, documentation or build configuration.
- Any other new functionality in `pkg/greeting`.

## Source References *(optional)*

- Parent issue: `handarbeit/fabrik-test-gamma#26` (regression scenario for #803 and #1308)
