# Feature Specification: Add HelloE2E202611260445532995 to pkg/greeting with a test

**Feature Branch**: `fabrik/issue-224`
**Created**: 2026-11-25
**Status**: Specified
**Input**: User description:

> Add one exported function and one test to the `pkg/greeting` package of this repository. Both the function name and the string literal below are per-run-unique and MUST be used exactly as written. Do not invent, re-derive or alter them. A downstream consumer's final assertion looks for this exact text in this repo.
>
> ## Requirements
>
> 1. In `pkg/greeting/greeting.go`, append an exported function:
>
>    ```go
>    // HelloE2E202611260445532995 returns the run-unique token for the
>    // 20261126-044553-2995 cross-repo spawn e2e run.
>    func HelloE2E202611260445532995() string {
>    	return "e2e-cross-repo-spawn-20261126-044553-2995"
>    }
>    ```
>
>    The file is an append-only list of one function per run. Follow the existing shape and place the new function after the last existing `HelloE2E…` function. Do not modify or remove any existing function.
>
> 2. In `pkg/greeting/greeting_test.go`, append a test that follows the existing per-function pattern:
>
>    ```go
>    func TestHelloE2E202611260445532995(t *testing.T) {
>    	const want = "e2e-cross-repo-spawn-20261126-044553-2995"
>    	if got := HelloE2E202611260445532995(); got != want {
>    		t.Errorf("HelloE2E202611260445532995() = %q, want %q", got, want)
>    	}
>    }
>    ```
>
> 3. Do not change the `go` directive or any dependency in `go.mod`. The consuming module targets `go 1.22`.
>
> ## Acceptance
>
> - `pkg/greeting/greeting.go` on this repo's default branch contains `HelloE2E202611260445532995` returning exactly `"e2e-cross-repo-spawn-20261126-044553-2995"`.
> - `pkg/greeting/greeting_test.go` contains `TestHelloE2E202611260445532995` and `go test ./pkg/greeting/...` passes.
> - All previously existing functions and tests are unchanged.
> - The PR is merged to the default branch, so that a consuming module can reference the commit.
>
> ## Scope
>
> Nothing beyond the function and its test. No other files or behaviour change.
>
> *Spawned by Fabrik from parent issue handarbeit/fabrik-test-gamma#2578 as a multi-issue decomposition. The parent's plan is at the link above.*

## Background

This issue is one child of a cross-repo spawn end-to-end run (parent: `handarbeit/fabrik-test-gamma#2578`). The run checks that Fabrik can decompose work into an issue in a different repository, drive that issue through the pipeline, and merge it. A consuming module in the parent repository then asserts that this repository's default branch contains a run-unique token.

`pkg/greeting` is an append-only list with one function and one test per run. This run adds a function named for this run's identifier (`20261126-044553-2995`). It returns a string built from the same identifier. Both the name and the string must match the issue text exactly, because the downstream assertion searches for that exact text. Any change to either value would make the run fail without a visible error.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Run-unique token is present on the default branch (Priority: P1)

A downstream consumer (the parent run's final assertion) looks in this repository's default branch for the run-unique function and its returned token.

**Why this priority**: This is the only deliverable. The parent run cannot complete without it.

**Independent Test**: Check out the default branch after the PR merges. Confirm `pkg/greeting/greeting.go` defines `HelloE2E202611260445532995` returning `"e2e-cross-repo-spawn-20261126-044553-2995"`. Run `go test ./pkg/greeting/...`.

**Acceptance Scenarios**:

1. **Given** the merged default branch, **When** `pkg/greeting/greeting.go` is searched for `HelloE2E202611260445532995`, **Then** exactly one exported function with that name exists and it returns `"e2e-cross-repo-spawn-20261126-044553-2995"`.
2. **Given** the merged default branch, **When** `go test ./pkg/greeting/...` is run, **Then** it passes, including `TestHelloE2E202611260445532995`.
3. **Given** the merged default branch, **When** it is compared with the pre-change branch, **Then** every previously existing function and test is byte-for-byte unchanged and the new function and test come after the last existing `HelloE2E…` entries.

---

### Edge Cases

- The name or string is mistyped, re-derived or reformatted: the downstream assertion fails. Both must be copied exactly as written.
- The `go` directive or a dependency in `go.mod` changes: the consuming module, which targets `go 1.22`, could stop building. `go.mod` must stay untouched.
- The new function is inserted before the last existing `HelloE2E…` function, or an existing function is edited: this breaks the append-only convention. It must be appended after the last existing one.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: `pkg/greeting/greeting.go` MUST contain an exported function `HelloE2E202611260445532995` that takes no arguments and returns exactly the string `"e2e-cross-repo-spawn-20261126-044553-2995"`.
- **FR-002**: The function MUST carry a doc comment of the form given in the Input, and MUST be placed after the last existing `HelloE2E…` function in the file.
- **FR-003**: `pkg/greeting/greeting_test.go` MUST contain a test `TestHelloE2E202611260445532995` that asserts the function's return value equals `"e2e-cross-repo-spawn-20261126-044553-2995"`, following the file's existing per-function test pattern, appended after the existing tests.
- **FR-004**: No existing function or test in `pkg/greeting` may be modified or removed.
- **FR-005**: `go.mod` (the `go` directive and all dependencies) MUST NOT change.
- **FR-006**: No file other than `pkg/greeting/greeting.go` and `pkg/greeting/greeting_test.go` may change in behaviour. The only other change allowed is the engine's own spec file under `specs/`.
- **FR-007**: The change MUST be merged to the repository's default branch so a consuming module can reference the commit.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: A text search of the default branch for `HelloE2E202611260445532995` finds the function in `pkg/greeting/greeting.go` and the test in `pkg/greeting/greeting_test.go`, and nowhere else outside the engine's spec file.
- **SC-002**: `go test ./pkg/greeting/...` passes on the default branch after the merge, with zero failing or skipped tests in the package.
- **SC-003**: The diff to `pkg/greeting` contains only added lines. The diff to `go.mod` is empty.
- **SC-004**: The downstream consumer's assertion for `e2e-cross-repo-spawn-20261126-044553-2995` succeeds against the merged commit.

## Assumptions

- `pkg/greeting/greeting.go` and `pkg/greeting/greeting_test.go` already exist and already follow the one-function-per-run, append-only pattern the issue describes.
- The identifier `20261126-044553-2995` and the compact form `202611260445532995` are intentionally paired, and both are used exactly as given.
- The `fabrik:yolo` label on this issue means the stage advances without a human approving the spec first, which is expected here.

## Out of Scope

- Any change to other files, packages, behaviour or dependencies.
- Refactoring, reformatting or deduplicating the existing `HelloE2E…` functions.
- Changes to the parent repository or the consuming module. This issue only makes the token available on this repository's default branch.
