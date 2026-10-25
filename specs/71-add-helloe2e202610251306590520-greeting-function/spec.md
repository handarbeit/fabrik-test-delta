# Feature Specification: Add HelloE2E202610251306590520 greeting function (e2e cross-repo spawn)

**Feature Branch**: `fabrik/issue-71`
**Created**: 2026-10-25
**Status**: Specified
**Input**: User description:

> ## Summary
>
> Add one new exported function to the `greeting` package, plus a test. This is a purely additive change. It is the delta-side prerequisite of an end-to-end cross-repo spawn regression scenario, in which the consumer repo `handarbeit/fabrik-test-gamma` will later import this function. That consumer work is out of scope for this issue.
>
> ## Requirements
>
> - **R1.** In `pkg/greeting/greeting.go`, add an exported function `HelloE2E202610251306590520() string`. It returns exactly the literal string `e2e-cross-repo-spawn-20261025-130659-0520`. Follow the shape of the existing `HelloE2E…` functions in the file (a doc comment, then a one-line body returning the literal). Append it after the last existing function.
> - **R2.** In `pkg/greeting/greeting_test.go`, add `TestHelloE2E202610251306590520`. Follow the existing per-function test pattern: `const want = "e2e-cross-repo-spawn-20261025-130659-0520"`, call the function, and fail with `t.Errorf("HelloE2E202610251306590520() = %q, want %q", got, want)` if they differ. Append it after the last existing test.
> - **R3.** The function name and the string literal must be used exactly as written above. They contain a per-run-unique token that a downstream assertion checks character for character. Do not shorten, regenerate, reformat or substitute them.
>
> ## Acceptance
>
> 1. Calling `HelloE2E202610251306590520()` returns exactly `e2e-cross-repo-spawn-20261025-130659-0520`.
> 2. `go build ./...` and `go test ./...` pass, including the new test.
> 3. After merge, the delta default branch contains both the function name and the literal.
>
> ## Out of scope
>
> - Changing or removing any existing greeting function or its test.
> - Any change in other repositories. Gamma's consumption of this function is handled separately.
>
> ## Notes for the pipeline
>
> Use the repository's default branch as the base. No other repository's unmerged work is needed. Add no new dependencies.
>
> *Spawned by Fabrik from parent issue handarbeit/fabrik-test-gamma#1005 as a multi-issue decomposition. The parent's plan is at the link above.*

## Background

The `greeting` package in `fabrik-test-delta` already carries a series of sentinel functions (`HelloE2E…`), each returning a fixed, run-unique string. They exist so that an end-to-end regression scenario can prove a cross-repo spawn works. The consumer repo `handarbeit/fabrik-test-gamma` imports a delta function, and a downstream assertion checks the function name and returned string character for character.

This issue adds the delta-side half for the current run, spawned from parent issue `handarbeit/fabrik-test-gamma#1005`. The new function must exist on delta's default branch before gamma can consume it. Gamma's consumption is separate work.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Expose the run-unique sentinel from the greeting package (Priority: P1)

A consumer of the `greeting` package (the gamma repo, later) can call `HelloE2E202610251306590520()` and get back the run-unique sentinel string.

**Why this priority**: This is the entire deliverable. Gamma's later work depends on it.

**Independent Test**: Call `HelloE2E202610251306590520()` and compare the result with `e2e-cross-repo-spawn-20261025-130659-0520`. The new unit test does exactly this.

**Acceptance Scenarios**:

1. **Given** the `greeting` package on the merged default branch, **When** `HelloE2E202610251306590520()` is called, **Then** it returns exactly `e2e-cross-repo-spawn-20261025-130659-0520`.
2. **Given** the change is applied, **When** `go build ./...` and `go test ./...` run, **Then** both pass, including `TestHelloE2E202610251306590520`.
3. **Given** the change has merged, **When** the default branch is searched, **Then** it contains both the function name and the literal string.

---

### Edge Cases

- The function name and the literal embed a per-run-unique token that is checked character for character downstream. Any typo, shortening, reformatting or regeneration breaks the downstream assertion even if delta's own tests pass.
- The new function must not collide with, shadow or alter any existing `HelloE2E…` or `HelloIssue…` function or test.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: The `greeting` package exports a function named exactly `HelloE2E202610251306590520` that takes no arguments and returns a `string`.
- **FR-002**: `HelloE2E202610251306590520()` returns exactly the string `e2e-cross-repo-spawn-20261025-130659-0520`.
- **FR-003**: The function follows the shape of the existing `HelloE2E…` functions: a doc comment, then a one-line body returning the literal. It is placed after the last existing function in its file.
- **FR-004**: The package's test file contains `TestHelloE2E202610251306590520`, following the existing per-function pattern. It declares `const want = "e2e-cross-repo-spawn-20261025-130659-0520"`, calls the function, and fails with `t.Errorf("HelloE2E202610251306590520() = %q, want %q", got, want)` when the result differs. It is placed after the last existing test.
- **FR-005**: The function name and string literal are used exactly as written in FR-001 and FR-002, with no shortening, regeneration, reformatting or substitution.
- **FR-006**: No existing function or test in the package is changed or removed, and no new dependency is added.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: `go build ./...` and `go test ./...` pass on the change, and the new test is among the passing tests.
- **SC-002**: After merge, a search of delta's default branch finds both `HelloE2E202610251306590520` and `e2e-cross-repo-spawn-20261025-130659-0520`, each spelled identically to this spec.
- **SC-003**: The diff is purely additive: no existing line of the package or its tests is modified or deleted.

## Assumptions

- The package lives where the existing `HelloE2E…` functions live. The issue names it as the `greeting` package under `pkg/greeting`, with a matching test file.
- The doc comment wording follows the most recent `HelloE2E…` entries ("returns the sentinel string for the cross-repo spawn regression test.").
- The repository's default branch (`main`) is the base. No other repository's unmerged work is required.

## Out of Scope *(optional)*

- Changing or removing any existing greeting function or its test.
- Any change in other repositories, including gamma's consumption of this function.
- Adding dependencies.

## Source References *(optional)*

- Parent issue: `handarbeit/fabrik-test-gamma#1005` (multi-issue decomposition that spawned this issue).
