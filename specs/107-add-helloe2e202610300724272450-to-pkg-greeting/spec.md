# Feature Specification: Add HelloE2E202610300724272450 sentinel to pkg/greeting

**Feature Branch**: `fabrik/issue-107`
**Created**: 2026-10-30
**Status**: Specified
**Input**: User description:

> ## Problem
>
> The `pkg/greeting` package needs a new exported sentinel function used by an end-to-end regression scenario for cross-repo decomposition. A downstream repo (`handarbeit/fabrik-test-gamma`) will import and call this function after this change is merged, so it must be on this repo's default branch.
>
> ## Requirements
>
> - **FR-001**: Add an exported function `HelloE2E202610300724272450() string` to `pkg/greeting/greeting.go`.
> - **FR-002**: The function MUST return exactly the string literal `e2e-cross-repo-spawn-20261030-072427-2450`.
> - **FR-003**: Add a test for it in `pkg/greeting/greeting_test.go` that asserts the exact return value and passes.
> - **FR-004**: Use the function name and string literal verbatim. Do not rename, normalise, paraphrase or substitute them. A per-run-unique token (`20261030-072427-2450`) is already baked into both, and the scenario's final assertion checks that this exact text lands on this repo's default branch.
>
> ## Implementation guidance
>
> Follow the existing pattern in `pkg/greeting/greeting.go`. Each earlier run appended a function of this shape:
>
> ```go
> // HelloE2E202610261605192109 returns the sentinel string for the cross-repo spawn regression test.
> func HelloE2E202610261605192109() string {
>     return "e2e-cross-repo-spawn-20261026-160519-2109"
> }
> ```
>
> Add the new function in the same style, after the existing ones. In `pkg/greeting/greeting_test.go`, add one test following the existing pattern (`const want = "..."; if got := F(); got != want { t.Errorf(...) }`). Make no other changes: no refactoring or cleanup of existing code, and no other behaviour.
>
> ## Acceptance
>
> 1. `pkg/greeting/greeting.go` on the default branch contains `HelloE2E202610300724272450() string` returning `"e2e-cross-repo-spawn-20261030-072427-2450"`.
> 2. `go test ./pkg/greeting/...` passes, including the new test.
> 3. `go build ./...` and `go vet ./...` succeed.
>
> ## Out of scope
>
> - Any change outside `pkg/greeting/greeting.go` and `pkg/greeting/greeting_test.go`.
> - Any change to the downstream repo that consumes this function. That is tracked separately and happens after this issue merges.
>
> *Spawned by Fabrik from parent issue handarbeit/fabrik-test-gamma#1319 as a multi-issue decomposition. The parent's plan is at the link above.*

## Background

An end-to-end regression scenario exercises cross-repo decomposition. A parent issue in `handarbeit/fabrik-test-gamma` (#1319) spawned this issue in this repo. After this change merges, a downstream repo (`handarbeit/fabrik-test-gamma`) will import and call a new exported sentinel function from `pkg/greeting`. The scenario's final assertion checks that the exact function name and string literal exist on this repo's default branch (`main`). Earlier runs of the scenario each appended one function of the same shape (for example `HelloE2E202610261605192109`), so this change continues an established pattern. The identifier carries a per-run-unique token (`20261030-072427-2450`), so the text must be reproduced exactly.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Downstream repo can call the new sentinel (Priority: P1)

The downstream repo imports `pkg/greeting` and calls `HelloE2E202610300724272450()`. It receives the run's unique sentinel string, which proves the cross-repo spawn delivered the change to this repo's default branch.

**Why this priority**: This is the whole purpose of the issue. The scenario cannot pass without it.

**Independent Test**: Run `go test ./pkg/greeting/...` and confirm the new test passes. Inspect `pkg/greeting/greeting.go` on `main` and confirm the function exists with the exact name and literal.

**Acceptance Scenarios**:

1. **Given** the change is merged to `main`, **When** a consumer calls `greeting.HelloE2E202610300724272450()`, **Then** it returns exactly `e2e-cross-repo-spawn-20261030-072427-2450`.
2. **Given** the change is on the branch, **When** `go test ./pkg/greeting/...` runs, **Then** all tests pass, including the new test that asserts the exact return value.
3. **Given** the change is on the branch, **When** `go build ./...` and `go vet ./...` run, **Then** both succeed.

---

### Edge Cases

- The name or literal is mistyped, normalised or paraphrased (for example a different timestamp digit, case change or added whitespace). The scenario's final assertion fails, so the text must be copied verbatim.
- Existing sentinel functions and tests in the package must remain unchanged and keep passing.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: `pkg/greeting/greeting.go` MUST define an exported function `HelloE2E202610300724272450() string`, placed after the existing functions and in the same style, with a doc comment like the earlier sentinels.
- **FR-002**: The function MUST return exactly the string literal `e2e-cross-repo-spawn-20261030-072427-2450`.
- **FR-003**: `pkg/greeting/greeting_test.go` MUST contain one new test that asserts the function's exact return value, following the existing test pattern (a `want` constant compared to the function's result), and the test MUST pass.
- **FR-004**: The function name and string literal MUST be used verbatim, with no renaming, normalisation, paraphrasing or substitution.
- **FR-005**: No other changes are made: no refactoring or cleanup of existing code, and no other behaviour.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: On the default branch, `pkg/greeting/greeting.go` contains `HelloE2E202610300724272450() string` returning `"e2e-cross-repo-spawn-20261030-072427-2450"`.
- **SC-002**: `go test ./pkg/greeting/...` passes, including the new test.
- **SC-003**: `go build ./...` and `go vet ./...` succeed with no errors.
- **SC-004**: The diff touches only `pkg/greeting/greeting.go` and `pkg/greeting/greeting_test.go`, plus the engine-generated spec file under `specs/`.

## Assumptions

- The existing `pkg/greeting` files already contain earlier sentinel functions and tests to use as the style reference, as the issue states.
- The new function is appended after the existing ones. The order of sentinels carries no behavioural meaning.
- The engine-generated `specs/107-.../spec.md` file is not a change "outside" the two named files in the sense of the out-of-scope rule. It is the pipeline's own artifact.

## Out of Scope *(optional)*

- Any change outside `pkg/greeting/greeting.go` and `pkg/greeting/greeting_test.go`.
- Any change to the downstream repo (`handarbeit/fabrik-test-gamma`) that consumes this function. That is tracked separately and happens after this issue merges.
- Refactoring, cleanup or behaviour changes to existing greeting functions.
