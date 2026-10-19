# Feature Specification: Add HelloE2E202610192022130633 to pkg/greeting

**Feature Branch**: `fabrik/issue-45`
**Created**: 2026-10-19
**Status**: Specified
**Input**: User description: "## Goal

Add one exported function and a test to the greeting package so another repository (`handarbeit/fabrik-test-gamma`) can call it. This is a fixture for an end-to-end regression test of cross-repo spawn (#803 on-demand spawn-target repo init, #1308 per-run-unique fixture token). A per-run-unique token is already baked into the names below. Use the function name and the string literal exactly as given. Do not shorten, regenerate, reformat or normalise either of them.

## Requirements

1. In `pkg/greeting/greeting.go`, append an exported function named exactly `HelloE2E202610192022130633` with signature `() string` that returns exactly the string `e2e-cross-repo-spawn-20261019-202213-0633`.
   - Append it at the end of the file, after the existing `HelloE2E…` functions.
   - Follow the existing convention, including the doc comment: `// HelloE2E202610192022130633 returns the sentinel string for the cross-repo spawn regression test.`
2. In `pkg/greeting/greeting_test.go`, add a test for it. It must assert that `HelloE2E202610192022130633()` returns exactly `e2e-cross-repo-spawn-20261019-202213-0633`. Match the style of the existing per-function tests in that file.
3. `go build ./...` and `go test ./...` must pass.

## Scope

- Only the single function and its test. No refactors, no other functions, no unrelated edits.
- No changes to Fabrik configuration or to any other repository.

## Acceptance

- A search of the default branch finds `HelloE2E202610192022130633` and `e2e-cross-repo-spawn-20261019-202213-0633` exactly as written above.
- The new test passes together with the existing tests on the default branch after the change lands.

## Notes

- Another repository will pin the default-branch commit that contains this change, so the change must be merged to the default branch for the work to count as done."

## Background

This issue is a fixture for an end-to-end regression test of cross-repo spawn. It covers on-demand initialisation of the spawn-target repository (#803) and a per-run-unique fixture token (#1308). It was spawned from parent issue `handarbeit/fabrik-test-gamma#560` as part of a multi-issue decomposition.

The parent work in `handarbeit/fabrik-test-gamma` needs a symbol in this repository's greeting package that it can call. The function name and its returned string both carry a per-run-unique token. That makes it possible to check that the change reached the default branch and was pinned by the other repository, and to rule out a stale or look-alike symbol from an earlier run.

The change is not done until it is merged to the default branch (`main`), because the other repository pins the default-branch commit that contains it.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Cross-repo caller can use the sentinel function (Priority: P1)

A caller in `handarbeit/fabrik-test-gamma` imports the greeting package at a pinned default-branch commit and calls `HelloE2E202610192022130633()`. It gets back the run-unique sentinel string.

**Why this priority**: This is the whole purpose of the issue. The cross-repo spawn regression test depends on it.

**Independent Test**: Run the greeting package tests on the default branch after merge. The new test passes, and a search of the default branch finds both the function name and the string literal exactly as specified.

**Acceptance Scenarios**:

1. **Given** the change is merged to `main`, **When** `HelloE2E202610192022130633()` is called, **Then** it returns exactly `e2e-cross-repo-spawn-20261019-202213-0633`.
2. **Given** the change is merged to `main`, **When** the default branch is searched for `HelloE2E202610192022130633` and for `e2e-cross-repo-spawn-20261019-202213-0633`, **Then** both are found exactly as written.
3. **Given** the change is applied, **When** `go build ./...` and `go test ./...` are run, **Then** both succeed, including all existing tests.

---

### Edge Cases

- The function name and the string literal contain a per-run-unique token. Any shortening, regeneration, reformatting or normalisation (case, separators, whitespace) would break the cross-repo check, so both must be copied exactly.
- The new function must go after the existing `HelloE2E…` functions, at the end of the file, so that it follows the existing convention and does not disturb them.
- The test must assert the exact full string, not a prefix, substring or length.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: The greeting package MUST expose an exported function named exactly `HelloE2E202610192022130633` with signature `() string`.
- **FR-002**: `HelloE2E202610192022130633()` MUST return exactly the string `e2e-cross-repo-spawn-20261019-202213-0633`.
- **FR-003**: The function MUST be appended at the end of `pkg/greeting/greeting.go`, after the existing `HelloE2E…` functions, with the doc comment `// HelloE2E202610192022130633 returns the sentinel string for the cross-repo spawn regression test.`
- **FR-004**: `pkg/greeting/greeting_test.go` MUST contain a test that asserts `HelloE2E202610192022130633()` returns exactly `e2e-cross-repo-spawn-20261019-202213-0633`, in the style of the existing per-function tests in that file.
- **FR-005**: `go build ./...` and `go test ./...` MUST pass with the change applied.
- **FR-006**: The change MUST reach the default branch (`main`), so another repository can pin the commit that contains it.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: A search of the default branch finds `HelloE2E202610192022130633` and `e2e-cross-repo-spawn-20261019-202213-0633` exactly as written, each at least once.
- **SC-002**: The full test suite, including the new test, passes on the default branch after the change lands.
- **SC-003**: The diff contains only the one new function and its one new test, with no other functions, refactors or unrelated edits.
- **SC-004**: A caller in `handarbeit/fabrik-test-gamma` pinned to the resulting default-branch commit can call the function and receive the exact sentinel string.

## Assumptions

- The existing `HelloE2E…` functions and per-function tests in `pkg/greeting` define the convention to follow. Their exact style is a detail for later stages.
- "Default branch" means `main`, as stated in the repository context.
- Landing the change on `main` goes through the normal pipeline and PR merge. No special release or tagging step is needed.

## Out of Scope

- Any other functions, refactors or unrelated edits in the greeting package or elsewhere.
- Changes to Fabrik configuration.
- Changes to any other repository, including `handarbeit/fabrik-test-gamma`. Pinning the resulting commit is that repository's responsibility.
