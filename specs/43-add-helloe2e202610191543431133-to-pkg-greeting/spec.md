# Feature Specification: Add HelloE2E202610191543431133 to pkg/greeting

**Feature Branch**: `fabrik/issue-43`
**Created**: 2026-10-19
**Status**: Specified
**Input**: User description: "Add one exported function and a test to the greeting package. A consumer repository (`handarbeit/fabrik-test-gamma`) imports this package and will call the new function once this issue is merged. Use the function name and string literal below **exactly as written**. A per-run-unique token is baked into both. Do not rename, shorten or regenerate them. A downstream assertion depends on that exact text appearing in this repository's default branch. In `pkg/greeting/greeting.go`, add an exported function `HelloE2E202610191543431133() string` that returns exactly the string `e2e-cross-repo-spawn-20261019-154343-1133`. Follow the pattern of the existing `HelloE2E…` functions in that file: a short doc comment, then a one-line function. Append it after the last existing `HelloE2E…` function. In `pkg/greeting/greeting_test.go`, add `TestHelloE2E202610191543431133`, following the existing one-test-per-function pattern. Use `const want = "e2e-cross-repo-spawn-20261019-154343-1133"` and fail with `t.Errorf` if `HelloE2E202610191543431133()` does not return it. Do not change any existing function, test, or output. Out of scope: any change to existing greeting functions; any change to the consumer repository, which is handled separately and will bump its dependency pin after this issue is merged."

## Background

This issue was spawned by Fabrik from parent issue handarbeit/fabrik-test-gamma#508 as part of a cross-repo decomposition. The consumer repository `handarbeit/fabrik-test-gamma` imports the greeting package from this repository and intends to call a new function once it exists here. This issue delivers the provider side: one new exported sentinel function carrying a per-run-unique token, plus its test. A downstream assertion checks that the exact token text is present in this repository's default branch after merge, so the name and literal must be reproduced character for character.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Consumer can call the new sentinel function (Priority: P1)

A maintainer of the consumer repository wants to call `HelloE2E202610191543431133()` from the greeting package and receive the agreed token string.

**Why this priority**: This is the whole deliverable; the consumer's dependency bump is blocked on it.

**Independent Test**: Run the new unit test; it passes only if the function returns exactly `e2e-cross-repo-spawn-20261019-154343-1133`.

**Acceptance Scenarios**:

1. **Given** the greeting package on the default branch after merge, **When** a caller invokes `HelloE2E202610191543431133()`, **Then** it returns exactly `e2e-cross-repo-spawn-20261019-154343-1133`.
2. **Given** the repository after this change, **When** `go build ./...`, `go vet ./...` and `go test ./...` are run, **Then** all pass.

---

### User Story 2 - Existing behavior is untouched (Priority: P2)

A maintainer reviewing the change wants confidence that nothing existing was altered.

**Why this priority**: The change must be purely additive so other consumers are unaffected.

**Independent Test**: Review the diff; it contains only additions of the new function and the new test.

**Acceptance Scenarios**:

1. **Given** the diff of this change, **When** inspected, **Then** no existing function, test, or output is modified or removed.

---

### Edge Cases

- The name and literal must not be shortened, renamed or regenerated; any deviation breaks the downstream assertion.
- The new function is placed after the last existing `HelloE2E…` function and follows their style (short doc comment, one-line body).
- The literal must appear in `pkg/greeting` (function and test) on the default branch after merge.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: The greeting package MUST export a function `HelloE2E202610191543431133() string` that returns exactly the string `e2e-cross-repo-spawn-20261019-154343-1133`.
- **FR-002**: The function MUST follow the existing `HelloE2E…` pattern (short doc comment, then a one-line function) and be placed after the last existing `HelloE2E…` function in the package's main source file.
- **FR-003**: The package's test file MUST contain `TestHelloE2E202610191543431133`, following the existing one-test-per-function pattern, using `const want = "e2e-cross-repo-spawn-20261019-154343-1133"` and failing via `t.Errorf` if the function does not return it.
- **FR-004**: The change MUST NOT modify or remove any existing function, test, or output.
- **FR-005**: The repository MUST build, vet and pass its tests (`go build ./...`, `go vet ./...`, `go test ./...`) with the change applied.

### Key Entities *(if applicable)*

- **Sentinel function**: A uniquely named exported function whose return value is a per-run token, used by a downstream cross-repo check.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: `HelloE2E202610191543431133()` returns exactly `e2e-cross-repo-spawn-20261019-154343-1133`, verified by the new test.
- **SC-002**: The literal `e2e-cross-repo-spawn-20261019-154343-1133` is present in the greeting package on the default branch after this issue is merged.
- **SC-003**: Build, vet and test commands all succeed with zero failures.
- **SC-004**: The diff contains only additions (no changes to existing functions, tests, or output).

## Assumptions

- The existing `HelloE2E…` functions and tests in the greeting package provide the style pattern to mirror.
- The parent issue handarbeit/fabrik-test-gamma#508 is mentioned for context only; no closing keyword is used for it.

## Out of Scope *(optional)*

- Any change to existing greeting functions.
- Any change to the consumer repository, which will bump its dependency pin separately after this issue is merged.

## Source References *(optional)*

- Parent issue: handarbeit/fabrik-test-gamma#508 (mentioned for context)
