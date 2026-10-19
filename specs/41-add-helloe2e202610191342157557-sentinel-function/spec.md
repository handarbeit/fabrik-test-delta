# Feature Specification: Add HelloE2E202610191342157557 to pkg/greeting

**Feature Branch**: `fabrik/issue-41`
**Created**: 2026-10-19
**Status**: Specified
**Input**: User description: "Add one exported sentinel function and a test for it to the `pkg/greeting` package. Another repository (`handarbeit/fabrik-test-gamma`) will import this function after it lands, so it must be merged to the default branch (`main`) of this repo. In `pkg/greeting/greeting.go`, add an exported function `HelloE2E202610191342157557() string` that returns `e2e-cross-repo-spawn-20261019-134215-7557`, following the existing pattern of the earlier `HelloE2E…` sentinel functions and appended after the last one, with the doc comment `// HelloE2E202610191342157557 returns the sentinel string for the cross-repo spawn regression test.` In `pkg/greeting/greeting_test.go`, add `TestHelloE2E202610191342157557`, following the one-test-per-function pattern, asserting the exact returned value `e2e-cross-repo-spawn-20261019-134215-7557` (via a `const want`, reporting `HelloE2E202610191342157557() = %q, want %q` on mismatch), placed after the last existing test. The function name and string literal MUST be used verbatim: the timestamp segment is `20261019-134215` and the suffix is `7557`. A downstream assertion checks for this exact text in `pkg/greeting/greeting.go` on the default branch. Out of scope: any change to other functions in `pkg/greeting`, releasing or versioning this repo, changes to `go.mod`. Acceptance: the text `e2e-cross-repo-spawn-20261019-134215-7557` appears in `pkg/greeting/greeting.go` on the default branch, and `go build ./...` and `go test ./...` pass."

## Background

This issue was spawned by Fabrik from parent issue `handarbeit/fabrik-test-gamma#501` as part of a multi-issue decomposition. It belongs to a cross-repo spawn regression test: the parent, in another repository, depends on this repository exposing a uniquely named sentinel function. Once this change lands on `main`, `handarbeit/fabrik-test-gamma` imports the function and a downstream assertion checks for the exact sentinel text in `pkg/greeting/greeting.go` on the default branch. The change therefore only counts once it is merged to `main`, and the name and literal must match exactly. The `pkg/greeting` package already holds earlier `HelloE2E…` sentinel functions, each with its own test, and this change adds one more in the same style.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Expose the new sentinel function (Priority: P1)

A consumer in another repository (`handarbeit/fabrik-test-gamma`) needs to import `HelloE2E202610191342157557` from `pkg/greeting` and receive the agreed sentinel string.

**Why this priority**: This is the whole purpose of the issue. The downstream regression test cannot proceed without it.

**Independent Test**: Call `HelloE2E202610191342157557()` and compare the result with `e2e-cross-repo-spawn-20261019-134215-7557`. Separately, search `pkg/greeting/greeting.go` on `main` for that exact text.

**Acceptance Scenarios**:

1. **Given** the change is merged to `main`, **When** the function is called, **Then** it returns exactly `e2e-cross-repo-spawn-20261019-134215-7557`.
2. **Given** the change is merged to `main`, **When** `pkg/greeting/greeting.go` is searched for `e2e-cross-repo-spawn-20261019-134215-7557`, **Then** the text is found.

---

### User Story 2 - Cover the new function with a test (Priority: P1)

A maintainer wants a test that pins the exact returned value, so any accidental change to the sentinel is caught.

**Why this priority**: The package convention is one test per function, and the acceptance criteria require passing tests.

**Independent Test**: Run `go test ./pkg/greeting/...` and confirm that the new test runs and passes.

**Acceptance Scenarios**:

1. **Given** the new function and test are present, **When** `go test ./pkg/greeting/...` runs, **Then** all tests pass, including `TestHelloE2E202610191342157557`.
2. **Given** the function's return value is altered, **When** the tests run, **Then** the new test fails and reports the actual and expected values.

---

### Edge Cases

- A shortened, regenerated or otherwise altered name or literal (wrong timestamp segment or suffix) would break the downstream assertion even though the local tests could still pass. The text must be used exactly as given.
- The change landing on a branch other than `main` does not satisfy the downstream consumer.
- The new function must not disturb the existing `HelloE2E…` functions or their tests.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: `pkg/greeting/greeting.go` MUST export a function named `HelloE2E202610191342157557` with signature `() string`.
- **FR-002**: The function MUST return exactly `e2e-cross-repo-spawn-20261019-134215-7557`.
- **FR-003**: The function MUST follow the existing pattern of the earlier `HelloE2E…` sentinel functions, be appended after the last existing one, and carry the doc comment `HelloE2E202610191342157557 returns the sentinel string for the cross-repo spawn regression test.`
- **FR-004**: `pkg/greeting/greeting_test.go` MUST contain `TestHelloE2E202610191342157557`, placed after the last existing test, which asserts the exact returned value and reports both the actual and expected values on mismatch.
- **FR-005**: The function name and string literal MUST be used verbatim, with timestamp segment `20261019-134215` and suffix `7557`, and MUST NOT be shortened, regenerated or substituted.
- **FR-006**: The change MUST be merged to the default branch (`main`) of this repository.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: The text `e2e-cross-repo-spawn-20261019-134215-7557` appears in `pkg/greeting/greeting.go` on `main`.
- **SC-002**: `go build ./...` completes without errors.
- **SC-003**: `go test ./...` passes, including the new test and all existing tests.
- **SC-004**: `handarbeit/fabrik-test-gamma` can import `HelloE2E202610191342157557` from `pkg/greeting` after the merge.

## Assumptions

- The earlier `HelloE2E…` sentinel functions and their one-test-per-function tests already exist in `pkg/greeting`, and the new function follows their style.
- The module has no dependencies, so no `go.mod` or `go mod tidy` change is expected.
- Getting the change onto `main` follows the normal pipeline PR and merge flow.

## Out of Scope

- Any change to other functions in `pkg/greeting`.
- Releasing or versioning this repository.
- Changes to `go.mod`.

## Source References

- Parent issue: `handarbeit/fabrik-test-gamma#501` (multi-issue decomposition for the cross-repo spawn regression test)
