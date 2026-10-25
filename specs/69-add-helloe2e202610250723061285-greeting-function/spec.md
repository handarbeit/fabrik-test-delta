# Feature Specification: Add HelloE2E202610250723061285 greeting function and test (e2e cross-repo spawn)

**Feature Branch**: `fabrik/issue-69`
**Created**: 2026-10-25
**Status**: Specified
**Input**: User description: "## Goal

Add one exported function and a test to the `greeting` package. A downstream repo (`handarbeit/fabrik-test-gamma`) will import this function once it has landed on this repo's default branch, so the exact names and literal below must be used verbatim.

## Required change

### `pkg/greeting/greeting.go`

Add an exported function:

```go
// HelloE2E202610250723061285 returns the sentinel string for the cross-repo spawn regression test.
func HelloE2E202610250723061285() string {
	return \"e2e-cross-repo-spawn-20261025-072306-1285\"
}
```

Follow the style of the existing `HelloE2E<timestamp>()` functions in the same file (doc comment, single literal return). Do not modify or remove any existing function.

### `pkg/greeting/greeting_test.go`

Add a test following the existing per-function pattern:

```go
func TestHelloE2E202610250723061285(t *testing.T) {
	const want = \"e2e-cross-repo-spawn-20261025-072306-1285\"
	if got := HelloE2E202610250723061285(); got != want {
		t.Errorf(\"HelloE2E202610250723061285() = %q, want %q\", got, want)
	}
}
```

Match the existing test files' conventions for names and error formatting if they differ slightly from the snippet.

## Constraints

- The function name `HelloE2E202610250723061285` and the string literal `e2e-cross-repo-spawn-20261025-072306-1285` are fixed. They contain a per-run-unique token (`20261025-072306-1285`). Do not substitute, shorten or invent a different name or token. An end-to-end assertion looks for this exact text in this repo's default branch.
- Make no other changes to this repo.

## Acceptance criteria

- `pkg/greeting/greeting.go` on the default branch exports `HelloE2E202610250723061285() string`, returning exactly `e2e-cross-repo-spawn-20261025-072306-1285`.
- `pkg/greeting/greeting_test.go` contains `TestHelloE2E202610250723061285`, and `go test ./...` passes.
- The change is merged to the default branch."

## Background

This issue was spawned by Fabrik from parent issue `handarbeit/fabrik-test-gamma#817` as part of an end-to-end regression test of cross-repo child spawning. The parent work lives in a downstream repository (`handarbeit/fabrik-test-gamma`) that will import a function from this repository's `greeting` package once it has landed on this repository's default branch (`main`). This issue is that upstream half: it supplies the exported sentinel function and its test.

The function name and its returned string both embed a per-run-unique token (`20261025-072306-1285`). The end-to-end assertion looks for this exact text on the default branch to prove the cross-repo spawn worked end to end, so the name and literal are fixed and cannot be altered, shortened or replaced. The repository already contains similar `HelloE2E<timestamp>()` sentinel functions, each with a matching test; this change adds one more in the same style.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Downstream repo can consume the sentinel function (Priority: P1)

The downstream repo `handarbeit/fabrik-test-gamma` imports the `greeting` package from this repo's default branch and calls `HelloE2E202610250723061285()`, receiving the agreed sentinel string.

**Why this priority**: This is the whole purpose of the issue. Without the function on the default branch, the parent's cross-repo end-to-end assertion cannot pass.

**Independent Test**: On the default branch, call `greeting.HelloE2E202610250723061285()` and compare the result to `e2e-cross-repo-spawn-20261025-072306-1285`. The new test does exactly this, and `go test ./...` passes.

**Acceptance Scenarios**:

1. **Given** the change is merged to `main`, **When** a consumer calls `HelloE2E202610250723061285()`, **Then** it returns exactly `e2e-cross-repo-spawn-20261025-072306-1285`.
2. **Given** the change is merged to `main`, **When** the end-to-end assertion searches the default branch for the function name and the literal, **Then** both are found verbatim.

---

### User Story 2 - Existing behaviour is untouched (Priority: P2)

Existing `HelloE2E<timestamp>()` functions and their tests keep working exactly as before.

**Why this priority**: The change must be purely additive so that nothing already depending on this package is affected.

**Independent Test**: Run `go test ./...` and confirm every pre-existing test still passes, and that the diff shows no modified or removed existing function.

**Acceptance Scenarios**:

1. **Given** the change is applied, **When** `go test ./...` runs, **Then** all existing tests and the new test pass.
2. **Given** the change is applied, **When** the diff is reviewed, **Then** it contains only additions of the one function and its one test.

---

### Edge Cases

- If the existing functions or tests use slightly different doc-comment or error-formatting conventions than the snippets in the request, the new code follows the existing files' conventions while keeping the name and literal unchanged.
- The new test must be distinct from existing tests, so the name must not collide with any other symbol in the package.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: The `greeting` package exports a function named exactly `HelloE2E202610250723061285` with signature `func() string`.
- **FR-002**: `HelloE2E202610250723061285()` returns exactly the string `e2e-cross-repo-spawn-20261025-072306-1285`, as a single literal return.
- **FR-003**: The function has a doc comment, in the style of the existing `HelloE2E<timestamp>()` functions, stating it returns the sentinel string for the cross-repo spawn regression test.
- **FR-004**: The package's test file contains a test named exactly `TestHelloE2E202610250723061285` that asserts the function returns `e2e-cross-repo-spawn-20261025-072306-1285`, following the existing per-function test pattern and error formatting.
- **FR-005**: No existing function or test in the repository is modified or removed, and no other change is made to the repository.
- **FR-006**: The change is merged to the repository's default branch (`main`) so a downstream repo can import it.

### Key Entities *(if applicable)*

- **Sentinel function**: An exported function in the `greeting` package whose name and return value embed a per-run-unique token, used by an end-to-end test to verify the change reached the default branch.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: On the default branch, the `greeting` package exports `HelloE2E202610250723061285() string` returning exactly `e2e-cross-repo-spawn-20261025-072306-1285`.
- **SC-002**: The package test file contains `TestHelloE2E202610250723061285`, and `go test ./...` passes with zero failures.
- **SC-003**: The merged change adds only the one function and its one test, with no modified or removed existing code.
- **SC-004**: The end-to-end assertion in the parent flow finds the exact function name and literal on this repository's default branch.

## Assumptions

- The `greeting` package lives at `pkg/greeting/` with its implementation in `greeting.go` and tests in `greeting_test.go`, as stated in the request.
- The existing `HelloE2E<timestamp>()` functions are the style reference (doc comment plus single literal return) and the existing tests the pattern for the new test.
- The snippets in the request are authoritative for name and literal; only incidental style (comment wording, error format) may be adjusted to match neighbouring code.

## Out of Scope

- Any change to the downstream repo `handarbeit/fabrik-test-gamma`.
- Modifying, renaming or removing any existing function or test.
- Any other change to this repository, including documentation, configuration or dependencies.
- Choosing a different name or token than the one specified.

## Source References

- Parent issue: `handarbeit/fabrik-test-gamma#817` (multi-issue decomposition that spawned this issue)
