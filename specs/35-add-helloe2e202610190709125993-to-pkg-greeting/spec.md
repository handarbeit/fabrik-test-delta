# Feature Specification: Add HelloE2E202610190709125993 to pkg/greeting (e2e cross-repo spawn)

**Feature Branch**: `fabrik/issue-35`
**Created**: 2026-10-19
**Status**: Specified
**Input**: User description: "## Goal

Add one exported function to the greeting package, with a test. Another repo (`handarbeit/fabrik-test-gamma`) will import this function once it is on this repo's default branch, so this change must land first.

## Requirements

- In `pkg/greeting/greeting.go`, add an exported function `HelloE2E202610190709125993() string` that returns exactly the string literal `\"e2e-cross-repo-spawn-20261019-070912-5993\"`.
- In `pkg/greeting/greeting_test.go`, add a test that calls `HelloE2E202610190709125993()` and asserts it returns exactly `e2e-cross-repo-spawn-20261019-070912-5993`.
- Use the function name and the string literal exactly as written above. Do not alter, abbreviate or regenerate them. They contain a per-run-unique token, and a final assertion depends on this exact text appearing in this repo's default branch.
- Follow the existing style in `pkg/greeting` for the function and its test. Earlier `HelloE2E…` functions are the precedent.

## Out of Scope

- Any other change to this repo.
- Refactoring the greeting package.

## Acceptance

- `go build ./...` and `go test ./...` pass.
- `pkg/greeting/greeting.go` on the default branch contains `HelloE2E202610190709125993` and `e2e-cross-repo-spawn-20261019-070912-5993`.
- The new test exists and passes."

## Background

This issue was spawned from parent issue `handarbeit/fabrik-test-gamma#442` as part of a multi-issue decomposition (an end-to-end cross-repository spawn regression test). A repository other than this one (`handarbeit/fabrik-test-gamma`) will import a new sentinel function from this repository's greeting package. That import can only succeed once the function exists on this repository's default branch, so this change must land first.

The sentinel string embeds a per-run-unique token. A final assertion in the cross-repo test checks that this exact text appears in the default branch of this repository, so the function name and string literal must be reproduced character-for-character.

The greeting package already contains many earlier `HelloE2E…` sentinel functions, each with a matching test. They are the precedent for the style of this change.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Expose the new sentinel function (Priority: P1)

A consumer in another repository (`fabrik-test-gamma`) needs to call a sentinel function from this repository's greeting package and receive a known, run-unique string.

**Why this priority**: It is the whole purpose of the issue. The dependent work in the other repository is blocked until the function is on the default branch.

**Independent Test**: Call `HelloE2E202610190709125993()` from the greeting package and compare the result to `e2e-cross-repo-spawn-20261019-070912-5993`.

**Acceptance Scenarios**:

1. **Given** the greeting package on the default branch, **When** `HelloE2E202610190709125993()` is called, **Then** it returns exactly `e2e-cross-repo-spawn-20261019-070912-5993`.
2. **Given** the default branch, **When** its greeting source is searched, **Then** both the function name `HelloE2E202610190709125993` and the literal `e2e-cross-repo-spawn-20261019-070912-5993` are present.

---

### User Story 2 - Guard the function with a test (Priority: P1)

A maintainer wants an automated test that fails if the sentinel function is removed or its return value drifts.

**Why this priority**: The acceptance criteria require the test, and the existing sentinel functions each have one.

**Independent Test**: Run the package tests and confirm the new test exists and passes.

**Acceptance Scenarios**:

1. **Given** the new function, **When** the package tests run, **Then** a test for `HelloE2E202610190709125993` runs and passes.
2. **Given** the function returned a different string, **When** the package tests run, **Then** the new test fails.

---

### Edge Cases

- A typo, abbreviation or regenerated token in the function name or the string literal would silently break the downstream assertion, so both must be copied exactly.
- The new function must not collide with or alter any existing `HelloE2E…` function or test.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: The greeting package exports a function named exactly `HelloE2E202610190709125993` that takes no arguments and returns a string.
- **FR-002**: `HelloE2E202610190709125993()` returns exactly the string `e2e-cross-repo-spawn-20261019-070912-5993`.
- **FR-003**: The greeting package's tests include a test that calls `HelloE2E202610190709125993()` and asserts the result equals exactly `e2e-cross-repo-spawn-20261019-070912-5993`.
- **FR-004**: The function and its test follow the style of the existing `HelloE2E…` sentinel functions and tests in the package.
- **FR-005**: The function name and the string literal are used exactly as given in this spec and are not altered, abbreviated or regenerated.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: The project builds and its full test suite passes with the change applied.
- **SC-002**: After the change lands, the default branch contains both `HelloE2E202610190709125993` and `e2e-cross-repo-spawn-20261019-070912-5993` in the greeting package source.
- **SC-003**: The new test exists and passes.
- **SC-004**: The dependent repository can import and call the function once the change is on the default branch.

## Assumptions

- The default branch is `main`, and the change lands there through the normal PR flow.
- The existing `HelloE2E…` functions and their tests are the style precedent, and following them satisfies "existing style".
- No other file or package in this repository needs to change.

## Out of Scope *(optional)*

- Any other change to this repository.
- Refactoring the greeting package.
- The import and use of this function in `handarbeit/fabrik-test-gamma`, which is handled by the parent issue's other work.

## Source References *(optional)*

- Parent issue: `handarbeit/fabrik-test-gamma#442`
- Precedent: the existing `HelloE2E…` sentinel functions and tests in the greeting package
