# Feature Specification: Add HelloE2E202610150650002855 greeting function

**Feature Branch**: `fabrik/issue-7`
**Created**: 2026-10-15
**Status**: Specified
**Input**: User description: "Add one new exported greeting function and a unit test for it to the `pkg/greeting` package. A downstream repo, `handarbeit/fabrik-test-gamma`, will import this function once it has merged. Add an exported function `HelloE2E202610150650002855() string` to `pkg/greeting/greeting.go`. It returns exactly the literal string `e2e-cross-repo-spawn-20261015-065000-2855`. Add a test for it to `pkg/greeting/greeting_test.go`. It asserts that the function returns exactly `e2e-cross-repo-spawn-20261015-065000-2855`. Follow the style of the existing `HelloE2E…` functions and tests in the same package. Use the function name and the string literal verbatim. Do not rename, regenerate or alter them. A per-run-unique token is already baked into both, and an external check looks for this exact text in this repo. Before adding the function, check that no function named `HelloE2E202610150650002855` already exists in the package. If one does, do not add a duplicate; make sure its return value and test match the requirement."

## Background

This issue was spawned from a parent issue in `handarbeit/fabrik-test-gamma` (#100) as part of a multi-issue decomposition. The downstream module needs a commit of this repository that contains a new greeting function, so it can depend on it after this change merges. The function name and return string embed a per-run-unique token, and an external check looks for that exact text in this repo. The text must therefore be used verbatim.

The `pkg/greeting` package already holds many similar `HelloE2E…` sentinel functions, each with a matching test. At the time of specification, no function named `HelloE2E202610150650002855` exists in the package; the name appears only in this issue's own context.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Downstream module can consume the new greeting function (Priority: P1)

A downstream module (`handarbeit/fabrik-test-gamma`) depends on a commit of this repository that exposes `HelloE2E202610150650002855` from the `pkg/greeting` package and receives the expected string.

**Why this priority**: This is the sole purpose of the issue. Nothing else in the change has value without it.

**Independent Test**: Call `greeting.HelloE2E202610150650002855()` from a test and compare the result to the expected literal. Run the package's test suite.

**Acceptance Scenarios**:

1. **Given** the change is merged to the default branch, **When** a consumer calls `HelloE2E202610150650002855()`, **Then** it returns exactly `e2e-cross-repo-spawn-20261015-065000-2855`.
2. **Given** the change is in place, **When** `go test ./...` is run, **Then** the new test is included and the whole suite passes.

---

### Edge Cases

- A function named `HelloE2E202610150650002855` already exists in the package (for example from an earlier attempt): no duplicate is added. The existing function's return value and its test are made to match the requirement.
- The name or literal has been altered, regenerated or "tidied" (for example a different timestamp or suffix): this is not acceptable, because the external check looks for the exact text.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: The `pkg/greeting` package exports a function `HelloE2E202610150650002855() string` that returns exactly the literal `e2e-cross-repo-spawn-20261015-065000-2855`.
- **FR-002**: The package's test file includes a test that asserts `HelloE2E202610150650002855()` returns exactly `e2e-cross-repo-spawn-20261015-065000-2855`.
- **FR-003**: The new function and test follow the style of the existing `HelloE2E…` functions and tests in the same package.
- **FR-004**: The function name and string literal are used verbatim, with no renaming, regeneration or alteration.
- **FR-005**: Before the function is added, the package is checked for an existing `HelloE2E202610150650002855`. If one exists, no duplicate is added, and the existing function's return value and test are made to match FR-001 and FR-002.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: Calling `HelloE2E202610150650002855()` returns `e2e-cross-repo-spawn-20261015-065000-2855`.
- **SC-002**: `go test ./...` passes and includes a test for the new function.
- **SC-003**: Exactly one function named `HelloE2E202610150650002855` exists in the package, and the exact name and literal text can be found in the repository.
- **SC-004**: The change is merged to the default branch (`main`), so a downstream module can depend on a commit that contains it.

## Assumptions

- The default branch is `main`.
- "Follow the existing style" means matching the shape of the neighbouring `HelloE2E…` functions and tests. No new patterns or abstractions are introduced.
- The `specs/<issue>-<slug>/spec.md` file the engine commits for this issue is separate from the "no other change" scope limit.

## Out of Scope

- Any change to the greeting package other than this one function and its test.
- Any change in other repositories. A follow-up in `fabrik-test-gamma` will consume this function after it merges.

## Source References

- Parent issue: `handarbeit/fabrik-test-gamma#100` (multi-issue decomposition)
