# Feature Specification: Add HelloE2E202610310144483284 greeting function and test

**Feature Branch**: `fabrik/issue-111`
**Created**: 2026-10-30
**Status**: Specified
**Input**: User description: "Add one exported function to the `greeting` package of this repository, with a test. A consumer repository (`handarbeit/fabrik-test-gamma`) will import it afterwards. This is a fixture for an end-to-end cross-repo regression scenario."

Original request, verbatim:

> ## Summary
>
> Add one exported function to the `greeting` package of this repository, with a test. A consumer repository (`handarbeit/fabrik-test-gamma`) will import it afterwards. This is a fixture for an end-to-end cross-repo regression scenario.
>
> ## Requirements
>
> 1. In `pkg/greeting/greeting.go`, add an exported function:
>
>    ```go
>    // HelloE2E202610310144483284 returns the sentinel string for the cross-repo spawn regression test.
>    func HelloE2E202610310144483284() string { return "e2e-cross-repo-spawn-20261031-014448-3284" }
>    ```
>
>    Append it after the existing `HelloE2E…` functions, following the same style as the existing sentinel functions in that file.
>
> 2. In `pkg/greeting/greeting_test.go`, add a test in the same style as the existing sentinel tests:
>
>    ```go
>    func TestHelloE2E202610310144483284(t *testing.T) {
>        const want = "e2e-cross-repo-spawn-20261031-014448-3284"
>        if got := HelloE2E202610310144483284(); got != want {
>            t.Errorf("HelloE2E202610310144483284() = %q, want %q", got, want)
>        }
>    }
>    ```
>
> 3. Use the function name `HelloE2E202610310144483284` and the string literal `e2e-cross-repo-spawn-20261031-014448-3284` exactly as written above. They contain a per-run-unique token. Do not paraphrase, regenerate or "correct" them, including the date segment. A downstream assertion checks for this exact text in this repository.
>
> ## Acceptance Criteria
>
> - `HelloE2E202610310144483284()` exists, is exported, and returns exactly `e2e-cross-repo-spawn-20261031-014448-3284`.
> - A test for it exists in `pkg/greeting/greeting_test.go` and passes.
> - `go build ./...` and `go test ./...` succeed.
> - Existing functions and tests are unchanged.
>
> ## Scope
>
> Only the one function and its test. Do not refactor or clean up the greeting package or its tests. If two concurrent changes both append at the end of the files, keep both additions.
>
> ## Notes
>
> Merge this to the default branch. The consumer repository bumps its Go module pin to a commit on this repository's default branch after this lands.

## Background

This issue was spawned from parent issue `handarbeit/fabrik-test-gamma#1475` as part of a multi-issue decomposition. It is a fixture for an end-to-end cross-repo regression scenario. This repository (`handarbeit/fabrik-test-delta`) must expose a new sentinel function in its `greeting` package. The consumer repository `handarbeit/fabrik-test-gamma` will then bump its Go module pin to a commit on this repository's default branch and import the function.

The function name and the string it returns contain a per-run-unique token. A downstream assertion looks for that exact text in this repository, so the text must not be altered in any way.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Expose the sentinel function (Priority: P1)

A consumer of the `greeting` package (the `fabrik-test-gamma` repository) can call `HelloE2E202610310144483284()` and receive the run-unique sentinel string.

**Why this priority**: This is the whole deliverable. The cross-repo scenario cannot proceed without the function on the default branch.

**Independent Test**: Call `HelloE2E202610310144483284()` and compare the result to `e2e-cross-repo-spawn-20261031-014448-3284`.

**Acceptance Scenarios**:

1. **Given** the `greeting` package on the default branch, **When** a consumer calls `HelloE2E202610310144483284()`, **Then** it returns exactly `e2e-cross-repo-spawn-20261031-014448-3284`.
2. **Given** the existing `HelloE2E…` sentinel functions, **When** the new function is added, **Then** it appears after them and follows the same style, and the existing functions are unchanged.

---

### User Story 2 - Test coverage for the sentinel (Priority: P1)

A test in the package's test file verifies the sentinel value, in the same style as the existing sentinel tests.

**Why this priority**: The acceptance criteria require a passing test. It guards against the literal being altered.

**Independent Test**: Run the package tests; `TestHelloE2E202610310144483284` is present and passes.

**Acceptance Scenarios**:

1. **Given** the test file, **When** the tests run, **Then** `TestHelloE2E202610310144483284` passes.
2. **Given** the build and the whole test suite, **When** `go build ./...` and `go test ./...` run, **Then** both succeed.

---

### Edge Cases

- If another concurrent change also appends to the end of `greeting.go` or `greeting_test.go`, both additions are kept. Neither is dropped to resolve the conflict.
- The date segment `20261031` in the literal looks inconsistent with the spec date. It is intentional and must not be "corrected".

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: `pkg/greeting/greeting.go` exposes an exported function named exactly `HelloE2E202610310144483284` that takes no arguments and returns a `string`.
- **FR-002**: `HelloE2E202610310144483284()` returns exactly the literal `e2e-cross-repo-spawn-20261031-014448-3284`.
- **FR-003**: The function carries the doc comment `// HelloE2E202610310144483284 returns the sentinel string for the cross-repo spawn regression test.` and is placed after the existing `HelloE2E…` functions, in the same style as them.
- **FR-004**: `pkg/greeting/greeting_test.go` contains `TestHelloE2E202610310144483284`. It asserts that the function returns the exact literal and reports a mismatch with `t.Errorf`, in the same style as the existing sentinel tests.
- **FR-005**: The function name and the string literal are used exactly as given. They are not paraphrased, regenerated or altered, including the date segment.
- **FR-006**: Existing functions and tests in the package are unchanged.
- **FR-007**: The change is limited to this one function and its test, with no refactoring or cleanup of the package or its tests.
- **FR-008**: The change is merged to the default branch (`main`) so the consumer repository can pin a commit on it.

### Key Entities

- **Sentinel function**: A function in the `greeting` package that returns a per-run-unique string. A downstream assertion in another repository checks for it.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: `go build ./...` and `go test ./...` both succeed on the change.
- **SC-002**: `TestHelloE2E202610310144483284` exists and passes.
- **SC-003**: A text search of the default branch for `HelloE2E202610310144483284` and `e2e-cross-repo-spawn-20261031-014448-3284` finds the exact text in both the source and test files.
- **SC-004**: The diff against the base branch contains only the added function and the added test, with no modifications or deletions to existing code.

## Assumptions

- The existing `HelloE2E…` functions and tests in the package define the "same style" to follow. The new code mirrors them.
- Merging to the default branch happens through the normal pipeline PR flow. The consumer repository's pin bump is separate work in `fabrik-test-gamma`.
- The literal's date segment is intentional, as the issue states.

## Out of Scope

- Refactoring or cleaning up the `greeting` package or its tests.
- Changes to the `fabrik-test-gamma` consumer repository, including bumping its Go module pin.
- Any behavior beyond returning the fixed string.

## Source References

- Parent issue: `handarbeit/fabrik-test-gamma#1475`
