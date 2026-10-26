# Feature Specification: Add HelloE2E202610261605192109 greeting function

**Feature Branch**: `fabrik/issue-105`
**Created**: 2026-10-26
**Status**: Specified
**Input**: User description: "Add one exported fixture function and its test to the `pkg/greeting` package. A consumer repo (`handarbeit/fabrik-test-gamma`) imports this function. It can't compile until this change is on this repo's default branch. This is part of an end-to-end regression for cross-repo decomposition (#803, #1308)."

## Background

A consumer repository, `handarbeit/fabrik-test-gamma`, imports a new exported function from this repository's `pkg/greeting` package. The consumer cannot compile until that function exists on this repository's default branch. This issue was spawned from the parent issue handarbeit/fabrik-test-gamma#1296 as part of an end-to-end regression for cross-repo decomposition (#803, #1308). A final assertion in that scenario checks the exact function name and literal on this repository's default branch.

The package already holds several `HelloE2E…` sentinel functions, each with its own per-function test. This change adds one more in the same style.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Consumer repo can import the new sentinel function (Priority: P1)

The consumer repo's code calls `greeting.HelloE2E202610261605192109()` and expects the run-unique string. Once this change is on the default branch, the consumer builds.

**Why this priority**: It is the only deliverable. The consumer repo's work depends on it, and the scenario's final assertion checks it.

**Independent Test**: On the default branch, call `HelloE2E202610261605192109()` and compare its result with `"e2e-cross-repo-spawn-20261026-160519-2109"`. Run `go build ./...` and `go test ./...`.

**Acceptance Scenarios**:

1. **Given** this change is on the default branch, **When** a caller invokes `HelloE2E202610261605192109()`, **Then** it returns exactly `"e2e-cross-repo-spawn-20261026-160519-2109"`.
2. **Given** this change is on the default branch, **When** `go test ./...` runs, **Then** `TestHelloE2E202610261605192109` exists and passes.
3. **Given** this change is on the default branch, **When** `go build ./...` runs, **Then** it succeeds.

---

### Edge Cases

- The name or literal is altered, shortened or regenerated. The final scenario assertion would then fail, so both must be used verbatim.
- The function is placed before an existing `HelloE2E…` function instead of after the last one. This breaks the ordering convention but not behaviour; the requirement still states "after the last existing one".

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: `pkg/greeting/greeting.go` exports a function named exactly `HelloE2E202610261605192109` with signature `func HelloE2E202610261605192109() string`, returning exactly the string `"e2e-cross-repo-spawn-20261026-160519-2109"`.
- **FR-002**: The function follows the style of the existing `HelloE2E…` functions in the file, including its doc comment, and is placed after the last existing one.
- **FR-003**: `pkg/greeting/greeting_test.go` contains a test named `TestHelloE2E202610261605192109` that asserts the function returns exactly `"e2e-cross-repo-spawn-20261026-160519-2109"`, in the style of the existing per-function tests (a `const want` and a `got != want` check using `t.Errorf`).

### Key Entities *(if applicable)*

- **HelloE2E202610261605192109**: A run-unique exported sentinel function in `pkg/greeting`, imported by the consumer repo and checked by the scenario's final assertion.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: On the default branch, the exported function exists with the exact name and returns the exact literal.
- **SC-002**: The new test exists and passes.
- **SC-003**: `go build ./...` and `go test ./...` both pass for the whole module.
- **SC-004**: The change touches only the two files named above, with no other functions, files or dependencies.

## Assumptions

- The existing `HelloE2E…` functions and tests in `pkg/greeting` are the style reference. Following the most recent one is sufficient.
- "Default branch" means `main`.

## Out of Scope *(optional)*

- Any change to Fabrik engine behaviour.
- Any change to the consumer repo. That is tracked separately and depends on this change.

## Goal

Add one exported fixture function and its test to the `pkg/greeting` package. A consumer repo (`handarbeit/fabrik-test-gamma`) imports this function. It can't compile until this change is on this repo's default branch. This is part of an end-to-end regression for cross-repo decomposition (#803, #1308).

## Constraints

- Use the function name `HelloE2E202610261605192109` and the literal `e2e-cross-repo-spawn-20261026-160519-2109` verbatim. The token is already unique to this run. Do not rename, regenerate, shorten or alter it. A final assertion for this scenario checks this exact text on this repo's default branch.
- Make no other changes: no other functions, files or dependencies.

## Acceptance Criteria

- `pkg/greeting/greeting.go` on the default branch exports `HelloE2E202610261605192109() string` returning exactly `"e2e-cross-repo-spawn-20261026-160519-2109"`.
- `pkg/greeting/greeting_test.go` contains `TestHelloE2E202610261605192109`.
- `go build ./...` and `go test ./...` pass.

---

*Spawned by Fabrik from parent issue handarbeit/fabrik-test-gamma#1296 as a multi-issue decomposition. The parent's plan is at the link above.*
