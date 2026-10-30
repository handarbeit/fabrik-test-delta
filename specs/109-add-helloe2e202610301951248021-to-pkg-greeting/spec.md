# Feature Specification: Add HelloE2E202610301951248021 sentinel to pkg/greeting

**Feature Branch**: `fabrik/issue-109`
**Created**: 2026-10-30
**Status**: Specified
**Input**: User description: "Add one exported function and its test to the `pkg/greeting` package of this repo. This is the provider half of an end-to-end cross-repo decomposition regression test. A consumer repo (`handarbeit/fabrik-test-gamma`) will import and call the function after this change has merged to this repo's default branch."

## Background

This issue is the provider half of an end-to-end cross-repo decomposition regression test. It was spawned from parent issue handarbeit/fabrik-test-gamma#1392 as part of a multi-issue decomposition. The consumer repo, `handarbeit/fabrik-test-gamma`, will import and call the new function only after this change has merged to this repo's default branch (`main`), so that it can resolve the function with `go get`.

The test covers #803 (on-demand spawn-target repo init) and #1308 (per-run-unique fixture token). A per-run-unique token is already baked into the function name and the string literal. A later assertion depends on that exact text appearing in this repo, so both must be used verbatim and never invented, paraphrased or regenerated.

The `pkg/greeting` package already holds a series of sentinel functions named `HelloE2E*`. The most recent is `HelloE2E202610300724272450`. The new function follows the same shape.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Provide the cross-repo sentinel function (Priority: P1)

A consumer module (`handarbeit/fabrik-test-gamma`) needs to import `pkg/greeting` from this repo and call `HelloE2E202610301951248021()`. It expects the function to return the per-run-unique sentinel string, so the end-to-end test can prove the cross-repo decomposition worked.

**Why this priority**: It is the only deliverable of this issue, and the consumer-side work is blocked until it has merged.

**Independent Test**: Run `go test ./pkg/greeting/...` in this repo. The new test passes and asserts the exact return value. Separately, once merged, `go get` from another module resolves the function.

**Acceptance Scenarios**:

1. **Given** the change is applied, **When** `HelloE2E202610301951248021()` is called, **Then** it returns exactly `e2e-cross-repo-spawn-20261030-195124-8021`.
2. **Given** the change is applied, **When** `go build ./...` and `go test ./...` run, **Then** both succeed.
3. **Given** the change has merged to `main`, **When** another module runs `go get` for this repo, **Then** the function is resolvable.

---

### Edge Cases

- The function name and string literal must match the issue exactly. A typo or a regenerated token would break the later cross-repo assertion.
- The new function must not alter or reorder existing `HelloE2E*` functions or tests.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: `pkg/greeting/greeting.go` exports a function `HelloE2E202610301951248021() string` that returns exactly `e2e-cross-repo-spawn-20261030-195124-8021`.
- **FR-002**: The function is appended after the existing `HelloE2E*` functions and uses the same shape as `HelloE2E202610300724272450`. It has the doc comment `// HelloE2E202610301951248021 returns the sentinel string for the cross-repo spawn regression test.` and a body of `return "e2e-cross-repo-spawn-20261030-195124-8021"`.
- **FR-003**: `pkg/greeting/greeting_test.go` contains a test for the new function that asserts it returns exactly `e2e-cross-repo-spawn-20261030-195124-8021`, in the style of the existing tests in that file.
- **FR-004**: The function name and string literal are used verbatim as given and are not paraphrased or regenerated.
- **FR-005**: No other behaviour changes. Existing functions and tests are untouched.

### Key Entities *(if applicable)*

- **Sentinel function**: An exported function in `pkg/greeting` returning a fixed per-run-unique string, used by a consumer repo to prove cross-repo resolution.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: `go build ./...` and `go test ./...` pass in this repo with the change applied.
- **SC-002**: The new test passes, and the function returns exactly `e2e-cross-repo-spawn-20261030-195124-8021`.
- **SC-003**: The change is merged to the default branch `main`, so the consumer repo can resolve the function with `go get`.
- **SC-004**: The diff touches only `pkg/greeting/greeting.go` and `pkg/greeting/greeting_test.go` (plus the spec file the engine commits), and removes no existing lines.

## Assumptions

- The existing `HelloE2E*` functions follow the shape described in the issue, and the existing tests in `greeting_test.go` provide a style to follow.
- The default branch is `main`, and merging the PR there is sufficient for `go get` resolution.

## Out of Scope *(optional)*

- Any change to the `fabrik-test-gamma` repo. Its `main.go` change is handled separately and happens after this one lands.
- Any change to this repo beyond the single function and its test.
