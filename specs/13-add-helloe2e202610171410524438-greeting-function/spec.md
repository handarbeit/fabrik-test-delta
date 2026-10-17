# Feature Specification: Add HelloE2E202610171410524438 greeting sentinel function and test

**Feature Branch**: `fabrik/issue-13`
**Created**: 2026-10-17
**Status**: Specified
**Input**: User description:

> ## Goal
>
> Add one exported sentinel function to the `pkg/greeting` package, plus a test, so a downstream Go module can import it. This is an end-to-end regression fixture for cross-repo decomposition. The function name and string literal below contain a per-run-unique token and **must be used exactly as written**. Do not invent, shorten or substitute any other name or literal.
>
> ## Required changes
>
> ### `pkg/greeting/greeting.go`
> Append an exported function at the end of the file, following the shape of the existing `HelloE2E…` sentinel functions:
>
> ```go
> // HelloE2E202610171410524438 returns the sentinel string for the cross-repo spawn regression test.
> func HelloE2E202610171410524438() string {
> 	return "e2e-cross-repo-spawn-20261017-141052-4438"
> }
> ```
>
> The function name is `HelloE2E202610171410524438`. The returned literal is exactly `e2e-cross-repo-spawn-20261017-141052-4438`.
>
> ### `pkg/greeting/greeting_test.go`
> Append a test following the existing `TestHelloE2E<token>` pattern:
>
> ```go
> func TestHelloE2E202610171410524438(t *testing.T) {
> 	const want = "e2e-cross-repo-spawn-20261017-141052-4438"
> 	if got := HelloE2E202610171410524438(); got != want {
> 		t.Errorf("HelloE2E202610171410524438() = %q, want %q", got, want)
> 	}
> }
> ```
>
> If the existing tests use a slightly different but equivalent style, follow the file's style while keeping the same name, constant and assertion.
>
> ## Acceptance criteria
>
> - `HelloE2E202610171410524438() string` is exported from `pkg/greeting/greeting.go` and returns exactly `e2e-cross-repo-spawn-20261017-141052-4438`.
> - `pkg/greeting/greeting_test.go` contains `TestHelloE2E202610171410524438` and it passes.
> - `go build ./...` and `go test ./...` pass in this repo.
> - After the change is merged, the default branch contains the text `HelloE2E202610171410524438` and the literal `e2e-cross-repo-spawn-20261017-141052-4438`.
> - No other function names, literals or existing sentinel functions are changed or removed.
>
> ## Scope
>
> Only the two files above. Do not modify `go.mod` or add dependencies, and do not change any existing function.

## Background

This issue is one half of an end-to-end regression fixture for cross-repo decomposition: a parent issue in another repository (`fabrik-test-gamma`) spawns this child issue in `fabrik-test-delta`. The check passes only if the child's work lands on this repository's default branch with an exact, per-run-unique marker that the harness can find. The `pkg/greeting` package already holds many earlier sentinel functions of the same shape, each with a unique token embedded in its name and returned string. This issue adds the sentinel for the current run, so the identifiers must match the run's token exactly. A substituted or shortened name or literal would make the fixture fail.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Expose the run's sentinel from the greeting package (Priority: P1)

A downstream Go module (or the e2e harness) imports the greeting package and calls the sentinel function for this run. It receives the exact per-run string.

**Why this priority**: This is the whole deliverable. Without it the cross-repo decomposition fixture cannot confirm that the child work landed.

**Independent Test**: Call `HelloE2E202610171410524438()` and compare the result to `e2e-cross-repo-spawn-20261017-141052-4438`.

**Acceptance Scenarios**:

1. **Given** the greeting package on the default branch after merge, **When** a caller invokes `HelloE2E202610171410524438()`, **Then** it returns exactly `e2e-cross-repo-spawn-20261017-141052-4438`.
2. **Given** the repository after the change, **When** the build and the full test suite are run, **Then** both pass.

---

### User Story 2 - Guard the sentinel with a test (Priority: P1)

A maintainer or CI run executes the package tests. A dedicated test asserts the sentinel's exact return value, so a later accidental edit is caught.

**Why this priority**: The issue requires the test and the suite must stay green. It matches the convention that every sentinel has a matching test.

**Independent Test**: Run the package tests and confirm `TestHelloE2E202610171410524438` exists and passes.

**Acceptance Scenarios**:

1. **Given** the change is applied, **When** the package tests run, **Then** `TestHelloE2E202610171410524438` passes.
2. **Given** the sentinel returned a different value, **When** the tests run, **Then** that test fails and reports the actual and expected values.

---

### Edge Cases

- A name or literal with a transposed, truncated or substituted token would pass a self-consistent test but break the fixture, so the exact strings in the requirements below govern.
- Existing tests use two equivalent assertion styles (a `const want` with an `if got := …` check, and separate `got`/`want` variables). Either is acceptable as long as the test name, expected constant and assertion are preserved.
- The new function and test are added after all existing ones, so no existing sentinel is reordered, renamed or altered.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: The greeting package MUST export a function named exactly `HelloE2E202610171410524438` that takes no arguments and returns a string.
- **FR-002**: `HelloE2E202610171410524438()` MUST return exactly `e2e-cross-repo-spawn-20261017-141052-4438`.
- **FR-003**: The function MUST be appended after the existing sentinel functions and carry a doc comment in the same form as them: `HelloE2E202610171410524438 returns the sentinel string for the cross-repo spawn regression test.`
- **FR-004**: The greeting package's tests MUST include a test named exactly `TestHelloE2E202610171410524438` that asserts the function returns `e2e-cross-repo-spawn-20261017-141052-4438`, and that test MUST pass.
- **FR-005**: The test MUST follow the file's existing `TestHelloE2E<token>` convention and be appended after the existing tests.
- **FR-006**: No existing function, test, name or literal may be changed or removed.
- **FR-007**: The module definition and dependencies MUST NOT change, and no Go or module files other than the greeting source file and its test file may change. The engine-managed copy of this spec under `specs/` is a pipeline artifact and is exempt.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: After merge, the default branch contains the text `HelloE2E202610171410524438` and the literal `e2e-cross-repo-spawn-20261017-141052-4438`.
- **SC-002**: A full build and the full test suite both pass in this repository, with the new test among the passing tests.
- **SC-003**: Apart from the engine-managed `specs/` copy of this spec, the change touches exactly two files (the greeting source file and its test file), and the diff to them adds lines only, with no deletions or modifications to existing lines.

## Assumptions

- The two files named in the issue already exist and follow the sentinel pattern visible in the repository, so appending is sufficient.
- The test-helper imports the test file already uses cover the new test, so no new imports are needed.
- Appending at the end of each file is acceptable placement, per the issue's explicit instruction.

## Out of Scope

- Changes to the module definition or dependencies.
- Changes, refactoring or cleanup of any existing sentinel function or test, including consolidating or deduplicating them.
- Any change to the parent issue in `fabrik-test-gamma` or to other repositories.
- Documentation changes.

## Source References

- Spawned by Fabrik from parent issue `handarbeit/fabrik-test-gamma#134` as a multi-issue decomposition (the parent's plan holds the original decomposition).
