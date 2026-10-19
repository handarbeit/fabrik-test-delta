# Feature Specification: Add HelloE2E202610191305159466 greeting function with test

**Feature Branch**: `fabrik/issue-39`
**Created**: 2026-10-19
**Status**: Specified
**Input**: User description: "Add one exported function to the greeting package in this repo and a test for it. This is the foundation for a cross-repo e2e scenario in which a downstream repo (`handarbeit/fabrik-test-gamma`) calls the function. That downstream change is separate and is not required by anything here. In `pkg/greeting/greeting.go`, add an exported function `HelloE2E202610191305159466() string` that returns exactly the literal string `e2e-cross-repo-spawn-20261019-130515-9466`. In `pkg/greeting/greeting_test.go`, add a test for `HelloE2E202610191305159466` that asserts it returns exactly that string. Follow the pattern of the existing `HelloE2E...` functions and tests in the same files: append the new function and test alongside them, and do not alter existing functions. Use the function name and the string literal verbatim; they contain a per-run-unique token, so do not shorten, regenerate or substitute them. Note that the function name has the digits `202610191305159466`, while the literal has `20261019-130515-9466`."

## Background

This issue is the upstream half of a cross-repo e2e scenario. It was spawned by Fabrik from parent issue `handarbeit/fabrik-test-gamma#481` as part of a multi-issue decomposition. The greeting package in this repo gains one more sentinel function, and a downstream repo (`handarbeit/fabrik-test-gamma`) will later call it. The downstream change is tracked separately and nothing in this issue depends on it.

The sentinel's name and return value carry a per-run-unique token, so the downstream repo and the e2e checks can tell this run's function apart from those of earlier runs. The package already holds a series of earlier sentinel functions of the same shape, each with its own test. The new one follows that established pattern.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Expose the new sentinel function (Priority: P1)

A downstream consumer imports the greeting package and calls `HelloE2E202610191305159466()`. It gets the exact run-unique string back.

**Why this priority**: This is the whole deliverable. The downstream repo's cross-repo e2e check depends on this function existing with this exact name and value.

**Independent Test**: Call `HelloE2E202610191305159466()` and compare the result to `e2e-cross-repo-spawn-20261019-130515-9466`.

**Acceptance Scenarios**:

1. **Given** the greeting package on the default branch, **When** `HelloE2E202610191305159466()` is called, **Then** it returns exactly `e2e-cross-repo-spawn-20261019-130515-9466`.
2. **Given** the greeting package, **When** the existing `HelloE2E...` functions are called, **Then** they return the same values as before the change.

---

### User Story 2 - Test coverage for the new function (Priority: P1)

A maintainer runs the package's tests and sees a test that asserts the new function's exact return value.

**Why this priority**: The acceptance criteria require a test covering the new function, and the repo convention is one test per sentinel function.

**Independent Test**: Run `go test ./...`. The new test passes. Changing the function's return value makes it fail.

**Acceptance Scenarios**:

1. **Given** the new function and its test, **When** `go test ./...` runs, **Then** all tests pass, including the new one.
2. **Given** the new test, **When** the function returns anything other than the expected literal, **Then** the test fails.

---

### Edge Cases

- The function name uses the digits `202610191305159466` with no separators, while the returned literal uses `20261019-130515-9466` with hyphens. Neither may be normalised to match the other.
- The identifier and literal are run-unique, so they must not be shortened, regenerated or reused from an earlier sentinel.
- Existing sentinel functions and tests must remain byte-for-byte unchanged. The new ones are appended alongside them.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: The greeting package exports a function `HelloE2E202610191305159466() string` that returns exactly `e2e-cross-repo-spawn-20261019-130515-9466`.
- **FR-002**: The greeting package's test file contains a test for `HelloE2E202610191305159466` that asserts the exact return value from FR-001.
- **FR-003**: The new function and test follow the form and placement of the existing `HelloE2E...` functions and tests, appended alongside them.
- **FR-004**: No existing function or test in the greeting package is modified, renamed or removed.
- **FR-005**: The change builds, passes static checks and passes the full test suite (`go build ./...`, `go vet ./...`, `go test ./...`).
- **FR-006**: The change lands on the repository's default branch (`main`).

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: Calling `HelloE2E202610191305159466()` returns a string equal to `e2e-cross-repo-spawn-20261019-130515-9466`.
- **SC-002**: `go build ./...`, `go vet ./...` and `go test ./...` all exit successfully with the change applied.
- **SC-003**: The diff touches only the greeting package's source and test file, and only by adding the new function and test.
- **SC-004**: The change is present on `main`.

## Assumptions

- The pattern to follow is the existing run-unique sentinel functions in the same package: a doc comment, then a function returning a string literal, with one test per function.
- The downstream repo `handarbeit/fabrik-test-gamma` will consume the function by its exact name once this change has landed. Nothing here is blocked on that.

## Out of Scope

- Any other change to the greeting package.
- Any change in other repositories, including the downstream call in `handarbeit/fabrik-test-gamma`.
