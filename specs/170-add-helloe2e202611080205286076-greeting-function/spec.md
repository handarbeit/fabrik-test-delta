# Feature Specification: Add HelloE2E202611080205286076 to the greeting package

**Feature Branch**: `fabrik/issue-170`
**Created**: 2026-11-07
**Status**: Specified
**Input**: User description: "Add HelloE2E202611080205286076 greeting function (e2e cross-repo spawn 20261108-020528-6076). The greeting package in this repo (`pkg/greeting`) is consumed by another repo, `handarbeit/fabrik-test-gamma`. That repo's entry point must call a new exported function from this package. It can only do so after the function exists here and has been merged, so this change must land first. It is part of an end-to-end regression scenario for cross-repo decomposition (#803, on-demand spawn-target repo init, and #1308, per-run-unique fixture token). The scenario's final assertion searches this repo for the exact text below. Add an exported function named exactly `HelloE2E202611080205286076` to `pkg/greeting/greeting.go` that takes no arguments and returns the exact string `e2e-cross-repo-spawn-20261108-020528-6076`, with a test in `pkg/greeting/greeting_test.go`."

## Background

The `pkg/greeting` package in this repo is consumed by `handarbeit/fabrik-test-gamma`, whose entry point must call a new exported function from it. That call can only be written once the function exists here and has been merged, so this change has to land first.

This is one step of an end-to-end regression scenario for cross-repo decomposition (#803, on-demand spawn-target repo init, and #1308, per-run-unique fixture token). The scenario's final assertion searches this repo's default branch for the exact function name and string literal. Both must therefore appear verbatim, without shortening or re-tokenising.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Consumer repo can call the new greeting function (Priority: P1)

A developer, or the automated scenario, working in `handarbeit/fabrik-test-gamma` needs to call `greeting.HelloE2E202611080205286076()` and receive a fixed, known string.

**Why this priority**: It is the entire purpose of the change. The downstream repo's work is blocked until it merges.

**Independent Test**: Run `go test ./...` in this repo. The new test calls the function and asserts the exact return value.

**Acceptance Scenarios**:

1. **Given** the change is merged, **When** `greeting.HelloE2E202611080205286076()` is called, **Then** it returns exactly `e2e-cross-repo-spawn-20261108-020528-6076`.
2. **Given** the change is merged, **When** `go build ./...` and `go test ./...` run, **Then** both pass.
3. **Given** the change is merged, **When** the default branch is searched for `HelloE2E202611080205286076` and `e2e-cross-repo-spawn-20261108-020528-6076`, **Then** both are found.

---

### Edge Cases

- Mistyped or re-tokenised identifiers: the function name embeds the digits `20261108020528` followed by `6076`, and the literal uses hyphens (`20261108-020528-6076`). A transposed, dropped or substituted digit would silently break the downstream assertion.
- Existing `HelloE2E…` functions and their tests must remain untouched. Only an addition is in scope.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: The greeting package exports a function named exactly `HelloE2E202611080205286076` that takes no arguments and returns a `string`.
- **FR-002**: The function returns exactly the literal string `e2e-cross-repo-spawn-20261108-020528-6076`.
- **FR-003**: A test for the function exists alongside the package's existing tests, follows their style, asserts the exact return value above, and passes.
- **FR-004**: The function name and string literal are used verbatim, with no shortening, re-tokenising or substitution.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: Calling the function returns exactly `e2e-cross-repo-spawn-20261108-020528-6076`.
- **SC-002**: The repo builds and its full test suite passes with the change applied.
- **SC-003**: Both `HelloE2E202611080205286076` and `e2e-cross-repo-spawn-20261108-020528-6076` are present in the default branch after merge.

## Assumptions

- The existing `HelloE2E…` functions in the greeting package are the pattern to follow for naming, shape and test style.
- The package file and its test file are the only files that need to change.

## Out of Scope *(optional)*

- Changing or removing any existing function.
- Changing any file other than the greeting package's source file and its test file.
- Any work in `handarbeit/fabrik-test-gamma`, which is a separate issue that depends on this one.

## Problem

The greeting package in this repo (`pkg/greeting`) is consumed by another repo, `handarbeit/fabrik-test-gamma`. That repo's entry point must call a new exported function from this package. It can only do so after the function exists here and has been merged, so this change must land first. It is part of an end-to-end regression scenario for cross-repo decomposition (#803, on-demand spawn-target repo init, and #1308, per-run-unique fixture token). The scenario's final assertion searches this repo for the exact text below.

## Requirements

- **FR-001**: Add an exported function named exactly `HelloE2E202611080205286076` to `pkg/greeting/greeting.go`. It takes no arguments and returns a `string`.
- **FR-002**: The function MUST return exactly the literal string `e2e-cross-repo-spawn-20261108-020528-6076`.
- **FR-003**: Add a test for it in `pkg/greeting/greeting_test.go`. It MUST assert that the function returns exactly `e2e-cross-repo-spawn-20261108-020528-6076`, and it MUST pass. Follow the style of the existing tests in that file.
- **FR-004**: Use the function name and string literal verbatim. Do not shorten, re-tokenise or substitute them. The function name contains the digits `20261108020528` followed by `6076`. The literal contains `20261108-020528-6076`, with hyphens.

## Acceptance Criteria

1. Calling `greeting.HelloE2E202611080205286076()` returns exactly `e2e-cross-repo-spawn-20261108-020528-6076`.
2. `go build ./...` and `go test ./...` pass in this repo.
3. The text `HelloE2E202611080205286076` and the literal `e2e-cross-repo-spawn-20261108-020528-6076` both appear in the default branch after merge.

## Scope

- Follow the pattern of the existing `HelloE2E…` functions in `pkg/greeting/greeting.go` and their tests.
- Do not change or remove any existing function, and do not change any other file.
- Only this one function and its test are in scope.

## Source References *(optional)*

- Parent issue: handarbeit/fabrik-test-gamma#1846
- Related scenario issues: #803 and #1308, mentioned for context

---

*Spawned by Fabrik from parent issue handarbeit/fabrik-test-gamma#1846 as a multi-issue decomposition. The parent's plan is at the link above.*
