# Feature Specification: Add HelloE2E202611170345385247 to pkg/greeting (e2e cross-repo spawn)

**Feature Branch**: `fabrik/issue-190`
**Created**: 2026-11-16
**Status**: Specified
**Input**: User description:

> ## Goal
>
> Add a new exported function and a test to the `greeting` package in this repo. Another repo will import the function, so it must exist on this repo's default branch with exactly the name and literal below.
>
> ## Requirements
>
> - In `pkg/greeting/greeting.go`, add an exported function `HelloE2E202611170345385247() string` that returns exactly the literal string `e2e-cross-repo-spawn-20261117-034538-5247`.
>   - Append it after the existing `HelloE2E...` functions.
>   - Follow the existing style, with a doc comment such as `// HelloE2E202611170345385247 returns the sentinel string for the cross-repo spawn regression test.`
> - In `pkg/greeting/greeting_test.go`, add `TestHelloE2E202611170345385247`.
>   - Follow the existing tests: declare `const want = "e2e-cross-repo-spawn-20261117-034538-5247"`, call the function, and fail with `t.Errorf` on a mismatch.
> - Use the function name and literal verbatim. Do not rename them, change their casing or alter the token. A per-run-unique token is baked into them, and a later assertion checks for this exact text in this repo.
>
> ## Acceptance Criteria
>
> - `HelloE2E202611170345385247()` returns exactly `e2e-cross-repo-spawn-20261117-034538-5247`.
> - `go test ./...` passes in this repo, including the new test.
> - The default branch contains both the exact function name and the exact literal once the change is merged.
>
> ## Scope
>
> Only the function and its test. Do not change any other behaviour or files in this repo.

## Background

This issue was spawned from a parent issue in another repository (`handarbeit/fabrik-test-gamma#2174`) as part of a multi-issue decomposition. It is part of an end-to-end regression test of cross-repo spawning. The parent work depends on this repo exposing a sentinel function in the `greeting` package. Another repo imports that function, and a later assertion checks that this repo's default branch contains the exact function name and literal. The name and literal embed a per-run-unique token, so any deviation (spelling, casing, digits) would break the downstream check. The `greeting` package already holds earlier sentinel functions of the same `HelloE2E...` family, each with its own test. This change adds one more in the same style.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Sentinel function is available to the importing repo (Priority: P1)

A consumer in another repo imports the `greeting` package from this repo's default branch and calls `HelloE2E202611170345385247()`. It gets the exact sentinel string.

**Why this priority**: This is the whole purpose of the issue. The cross-repo spawn test cannot pass without it.

**Independent Test**: Call `HelloE2E202611170345385247()` and compare the result with `e2e-cross-repo-spawn-20261117-034538-5247`. Search the merged default branch for both the function name and the literal.

**Acceptance Scenarios**:

1. **Given** the change is merged to the default branch, **When** a consumer calls `HelloE2E202611170345385247()`, **Then** it returns exactly `e2e-cross-repo-spawn-20261117-034538-5247`.
2. **Given** the change is merged to the default branch, **When** the repo is searched for the function name and the literal, **Then** both are found verbatim.

---

### User Story 2 - The sentinel is covered by a test (Priority: P2)

A maintainer running the package's test suite sees a dedicated test that fails if the function's return value ever deviates from the expected literal.

**Why this priority**: The test guards the literal against accidental edits, and the issue requires `go test ./...` to pass with it included.

**Independent Test**: Run `go test ./...` and confirm `TestHelloE2E202611170345385247` runs and passes. Temporarily changing the returned literal makes it fail.

**Acceptance Scenarios**:

1. **Given** the function and its test are present, **When** `go test ./...` runs, **Then** the whole suite passes, including `TestHelloE2E202611170345385247`.
2. **Given** the function returned a different string, **When** the test runs, **Then** it fails with an error reporting the mismatch.

---

### Edge Cases

- The function name, the test name and the literal contain a per-run-unique token. Any change in casing, spelling or digits invalidates the downstream assertion.
- Existing `HelloE2E...` functions and their tests must remain unchanged. The new function is appended after them.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: The `greeting` package MUST export a function `HelloE2E202611170345385247() string` that returns exactly the literal `e2e-cross-repo-spawn-20261117-034538-5247`.
- **FR-002**: The new function MUST be placed after the existing `HelloE2E...` functions in the package's main source file and MUST carry a doc comment in the style of the existing ones, such as `// HelloE2E202611170345385247 returns the sentinel string for the cross-repo spawn regression test.`
- **FR-003**: The package's test file MUST contain `TestHelloE2E202611170345385247`. Like the existing tests, it declares `const want = "e2e-cross-repo-spawn-20261117-034538-5247"`, calls the function, and reports a mismatch with `t.Errorf`.
- **FR-004**: The function name, test name and literal MUST be used verbatim, with no renaming, recasing or alteration of the token.
- **FR-005**: The change MUST NOT alter any other behaviour or files in the repo beyond the function and its test, apart from the engine's own spec-persistence file.

### Key Entities *(if applicable)*

- **Sentinel function**: A zero-argument exported function in the `greeting` package that returns a fixed, per-run-unique string. A separate repo imports it to verify cross-repo spawning.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: `HelloE2E202611170345385247()` returns exactly `e2e-cross-repo-spawn-20261117-034538-5247`.
- **SC-002**: The repo's full test suite (`go test ./...`) passes, including the new test.
- **SC-003**: After merge, the default branch contains both the exact function name and the exact literal, so the downstream assertion succeeds.
- **SC-004**: No existing function, test or other file is modified, apart from the engine's own spec-persistence file.

## Assumptions

- The `greeting` package already has existing `HelloE2E...` functions and tests whose style (doc comment, `const want`, `t.Errorf`) the new code follows.
- The repo's default branch is `main`, and the work reaches it through the normal PR and merge flow.
- The package's existing import path and package name stay as they are. Only an additive change is needed for the consumer.

## Out of Scope *(optional)*

- Any change to existing greeting functions or tests.
- Any change to behaviour, documentation, configuration or files outside the function and its test.
- The consumer-side work in the other repo that imports this function.

## Source References *(optional)*

- Parent issue: `handarbeit/fabrik-test-gamma#2174` (multi-issue decomposition that spawned this issue)

*Spawned by Fabrik from parent issue handarbeit/fabrik-test-gamma#2174 as a multi-issue decomposition. The parent's plan is at the link above.*
