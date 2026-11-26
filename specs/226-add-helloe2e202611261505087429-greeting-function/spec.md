# Feature Specification: Greeting function for e2e cross-repo spawn

**Feature Branch**: `fabrik/issue-226`
**Created**: 2026-11-26
**Status**: Specified
**Input**: User description: "Add HelloE2E202611261505087429 greeting function and test (e2e cross-repo spawn 20261126-150508-7429). `pkg/greeting/greeting.go` MUST define an exported function `HelloE2E202611261505087429() string` that returns exactly the string `e2e-cross-repo-spawn-20261126-150508-7429`, with a doc comment in the same style as the other `HelloE2E…` functions, appended after the existing ones. `pkg/greeting/greeting_test.go` MUST contain a test `TestHelloE2E202611261505087429` that verifies the function returns exactly that string, following the existing pattern (`const want = "…"` and `if got := HelloE2E202611261505087429(); got != want { t.Errorf(...) }`). Use the function name and string literal EXACTLY as written, verbatim; do not invent, normalise, shorten or regenerate the token."

## Background

A consumer repository (`handarbeit/fabrik-test-gamma`) will import this repository's `pkg/greeting` package and call a new exported function. The function must exist here and be merged to the default branch first, because the consumer cannot compile against a function that does not exist yet. This is an e2e regression scenario for cross-repo spawn. A per-run-unique token is baked into the function name and the returned string, and a later assertion looks for that exact text in this repo.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Downstream module can call the new greeting function (Priority: P1)

A downstream module fetches this repository's default branch and calls `HelloE2E202611261505087429()` from `pkg/greeting`. It must compile and return the run-unique sentinel string.

**Why this priority**: This is the only deliverable. The consumer repository cannot proceed until the function exists on the default branch.

**Independent Test**: Run the package tests in this repository. The new test passes, and calling the function returns exactly `e2e-cross-repo-spawn-20261126-150508-7429`.

**Acceptance Scenarios**:

1. **Given** the change is merged to the default branch, **When** a downstream module runs `go get` for this module, **Then** `greeting.HelloE2E202611261505087429` is available to call.
2. **Given** the function is called, **When** the result is compared with `e2e-cross-repo-spawn-20261126-150508-7429`, **Then** they are exactly equal.
3. **Given** the repository, **When** `go test ./...` is run, **Then** it passes, including `TestHelloE2E202611261505087429`.

---

### Edge Cases

- The token in the function name and the returned string must be used verbatim. Any normalisation, shortening or regeneration breaks the downstream assertion.
- Existing `HelloE2E…` functions and their tests must remain unchanged. The new function is appended after them.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: `pkg/greeting/greeting.go` MUST define an exported function `HelloE2E202611261505087429() string` that returns exactly the string `e2e-cross-repo-spawn-20261126-150508-7429`, with a doc comment in the same style as the other `HelloE2E…` functions already in the file, appended after the existing ones.
- **FR-002**: `pkg/greeting/greeting_test.go` MUST contain a test `TestHelloE2E202611261505087429` that verifies the function returns exactly `e2e-cross-repo-spawn-20261126-150508-7429`, following the file's existing pattern: `const want = "…"` and `if got := HelloE2E202611261505087429(); got != want { t.Errorf(...) }`.
- **FR-003**: The function name and string literal MUST be used exactly as written in FR-001 and FR-002, verbatim, with no invented, normalised, shortened or regenerated token.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: `go test ./...` passes in this repository, including the new test.
- **SC-002**: Calling `HelloE2E202611261505087429()` returns exactly `e2e-cross-repo-spawn-20261126-150508-7429`.
- **SC-003**: The change is merged to the default branch (`main`), so downstream modules can fetch it with `go get`.

## Assumptions

- The existing `HelloE2E…` functions in `pkg/greeting` define the doc-comment and test style to follow.
- The default branch is `main`, and merging there is how the function becomes fetchable downstream.

## Out of Scope *(optional)*

- Any other change in this repository beyond the one function and its test.
- Any change in the consumer repository (`handarbeit/fabrik-test-gamma`). That is handled separately and only after this change has merged.

## Source References *(optional)*

- Spawned by Fabrik from parent issue handarbeit/fabrik-test-gamma#2657 as a multi-issue decomposition. The parent's plan is at the link above.
