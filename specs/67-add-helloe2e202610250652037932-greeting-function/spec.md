# Feature Specification: Add HelloE2E202610250652037932 greeting function and test

**Feature Branch**: `fabrik/issue-67`
**Created**: 2026-10-25
**Status**: Specified
**Input**: User description:

> ## Goal
>
> Add one exported greeting function and its test to the greeting package, so that a downstream repository (`handarbeit/fabrik-test-gamma`) can import and call it. This is the delta-side half of an end-to-end cross-repo decomposition scenario. A per-run-unique token is baked into the names below, and the scenario's final assertion checks for this exact text in this repo.
>
> ## Requirements
>
> 1. In `pkg/greeting/greeting.go`, append an exported function at the end of the file, with a doc comment, following the style of the existing sentinel functions (for example `HelloE2E202610250615015471`):
>
>    ```go
>    func HelloE2E202610250652037932() string
>    ```
>
>    It MUST return exactly this string literal:
>
>    ```
>    e2e-cross-repo-spawn-20261025-065203-7932
>    ```
>
> 2. In `pkg/greeting/greeting_test.go`, append a test named `TestHelloE2E202610250652037932`, following the existing pattern: `const want = "e2e-cross-repo-spawn-20261025-065203-7932"`, then compare the function's return value `got` against `want`, and fail with `t.Errorf` on mismatch.
>
> 3. Use the function name `HelloE2E202610250652037932` and the string literal `e2e-cross-repo-spawn-20261025-065203-7932` VERBATIM. Do not rename, shorten, "improve" or substitute any token. A different token fails the scenario.
>
> ## Scope
>
> - Only the single function and its single test. Do not modify other functions, other files, `go.mod`, or CI configuration.
> - Do not add dependencies.
>
> ## Acceptance
>
> - `go build ./...` and `go test ./...` pass in this repo.
> - `HelloE2E202610250652037932()` returns exactly `e2e-cross-repo-spawn-20261025-065203-7932`.
> - A test covering the function exists in `pkg/greeting/greeting_test.go` and passes.
> - The change is merged to this repo's default branch so that other modules can fetch it by pseudo-version.
> - Write the PR body with `Closes #<this issue number>` for this issue only.
>
> ---
>
> *Spawned by Fabrik from parent issue handarbeit/fabrik-test-gamma#801 as a multi-issue decomposition. The parent's plan is at the link above.*

## Background

This issue is the delta-side half of an end-to-end cross-repo decomposition scenario. A parent issue in `handarbeit/fabrik-test-gamma` (#801) was decomposed into work in this repository. The downstream repository must be able to import and call a new greeting function from this repo's greeting package. The scenario's final assertion looks for the exact function name and exact string literal in this repo, so both tokens are fixed by the scenario and must not be altered. The greeting package already contains a series of similar per-run sentinel functions, each with a matching test; this change adds one more following that established pattern.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Downstream module can call the new sentinel (Priority: P1)

A downstream module (`handarbeit/fabrik-test-gamma`) fetches this repository at the default branch by pseudo-version and calls `HelloE2E202610250652037932()` from the greeting package, receiving the fixed sentinel string.

**Why this priority**: This is the whole purpose of the issue; the cross-repo scenario fails without it.

**Independent Test**: Run the greeting package tests in this repo; the new test passes. Calling the function from any importing module returns `e2e-cross-repo-spawn-20261025-065203-7932`.

**Acceptance Scenarios**:

1. **Given** the change is merged to the default branch, **When** another module imports the greeting package and calls `HelloE2E202610250652037932()`, **Then** it receives exactly `e2e-cross-repo-spawn-20261025-065203-7932`.
2. **Given** the repository after the change, **When** `go build ./...` and `go test ./...` are run, **Then** both succeed, including `TestHelloE2E202610250652037932`.

---

### Edge Cases

- A mistyped or "improved" token (different function name or string literal) would fail the scenario's final assertion; both must match the specification character for character.
- Existing sentinel functions and tests must remain unchanged; the new function and test are appended at the end of their respective files.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: The greeting package exposes an exported function named exactly `HelloE2E202610250652037932` that takes no arguments and returns a `string`.
- **FR-002**: `HelloE2E202610250652037932()` returns exactly the string `e2e-cross-repo-spawn-20261025-065203-7932`.
- **FR-003**: The function is appended at the end of the greeting source file and carries a doc comment in the same style as the existing sentinel functions.
- **FR-004**: A test named `TestHelloE2E202610250652037932` is appended at the end of the greeting test file, following the existing pattern: a constant `want` holding the expected string, a `got` value from the function, and a `t.Errorf` failure on mismatch.
- **FR-005**: No other functions, files, dependency declarations, or CI configuration are modified, and no dependencies are added.
- **FR-006**: The pull request body contains a closing reference for this issue only (`Closes #67`).

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: Building and testing the whole repository succeeds with zero failures after the change.
- **SC-002**: The new function's return value equals the specified string exactly (byte-for-byte), verified by the new test.
- **SC-003**: The change is merged to the default branch so a downstream module can fetch it by pseudo-version.
- **SC-004**: A search of the repository for the function name and the string literal finds them exactly as specified in the greeting source and test files, and nowhere else is changed.

## Assumptions

- The existing sentinel functions and tests in the greeting package (for example `HelloE2E202610250615015471`) define the style to follow.
- The fixed tokens given in the issue are authoritative and are not to be validated against or derived from any other source.

## Out of Scope

- Changes to any existing function, test, or file other than appending the one function and one test.
- Changes to `go.mod`, dependencies, or CI configuration.
- Work in `handarbeit/fabrik-test-gamma` (the downstream consumer), which is handled by its own issue.
