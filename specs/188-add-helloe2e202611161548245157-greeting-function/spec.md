# Feature Specification: Add HelloE2E202611161548245157 greeting function and test

**Feature Branch**: `fabrik/issue-188`
**Created**: 2026-11-16
**Status**: Specified
**Input**: User description: "## Problem

This is the beta-side half of an end-to-end cross-repo spawn regression scenario (#803 on-demand spawn-target repo init, #1308 per-run-unique fixture token). The alpha repo `handarbeit/fabrik-test-gamma` needs to import a new exported function from this repo's `pkg/greeting` package, and the function does not exist yet. This change must land first, because alpha cannot compile against a function that does not exist.

## Requirements

1. In `pkg/greeting/greeting.go`, add an exported function with exactly this name, taking no arguments and returning a string:

   `func HelloE2E202611161548245157() string`

   It must return exactly this literal: `e2e-cross-repo-spawn-20261116-154824-5157`

2. In `pkg/greeting/greeting_test.go`, add a test for `HelloE2E202611161548245157` that asserts the function returns exactly `e2e-cross-repo-spawn-20261116-154824-5157`. Follow the structure and naming used by the most recent existing `HelloE2E…` function and test in these files (one test per function, or a table entry, whichever the latest entries use).

3. Use the function name and string literal verbatim as given above. Do not invent substitute names or tokens. The scenario's final assertion checks for this exact text in this repo.

## Scope

- Only the one function and its test. Make no other changes to the greeting package or elsewhere in the repo.
- Do not touch the alpha repo (`handarbeit/fabrik-test-gamma`). Its change is tracked separately and depends on this one.

## Acceptance Criteria

- `HelloE2E202611161548245157()` is exported from `pkg/greeting` and returns exactly `e2e-cross-repo-spawn-20261116-154824-5157`.
- A passing unit test for the new function exists in `pkg/greeting/greeting_test.go`.
- `go build ./...` and `go test ./...` pass in this repo.
- The change is merged into this repo's default branch."

## Background

This issue is the beta-side half of an end-to-end cross-repo spawn regression scenario (#803 on-demand spawn-target repo init, #1308 per-run-unique fixture token). It was spawned from parent issue `handarbeit/fabrik-test-gamma#2096` as part of a multi-issue decomposition.

The alpha repo `handarbeit/fabrik-test-gamma` needs to import a new exported function from this repo's `pkg/greeting` package. That function does not exist yet, so alpha cannot compile against it. This change must therefore land first. The scenario's final assertion checks for the exact function name and token text in this repo, so both must be used verbatim.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Export the sentinel greeting function (Priority: P1)

A consumer of `pkg/greeting` (the alpha repo) can call `HelloE2E202611161548245157()` and receive the per-run-unique fixture token.

**Why this priority**: It is the whole change. Alpha's compilation and the scenario's final assertion depend on it.

**Independent Test**: Run `go test ./pkg/greeting/...` and confirm the new test passes. Confirm `go build ./...` succeeds.

**Acceptance Scenarios**:

1. **Given** the `pkg/greeting` package, **When** `HelloE2E202611161548245157()` is called with no arguments, **Then** it returns exactly `e2e-cross-repo-spawn-20261116-154824-5157`.
2. **Given** a different package in a different module that imports `pkg/greeting`, **When** it references `HelloE2E202611161548245157`, **Then** the reference compiles because the function is exported.

---

### Edge Cases

- A near-miss function name or token (different timestamp digits, altered casing or separators) would pass local tests but fail the scenario's final text assertion. Both must be byte-for-byte as specified.
- Earlier `HelloE2E…` functions and tests must stay unchanged and passing.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: `pkg/greeting/greeting.go` exports a function named exactly `HelloE2E202611161548245157`, taking no arguments and returning a `string`.
- **FR-002**: `HelloE2E202611161548245157()` returns exactly the literal `e2e-cross-repo-spawn-20261116-154824-5157`.
- **FR-003**: `pkg/greeting/greeting_test.go` contains a test for `HelloE2E202611161548245157` asserting that it returns exactly `e2e-cross-repo-spawn-20261116-154824-5157`.
- **FR-004**: The new function and test follow the structure and naming of the most recent existing `HelloE2E…` function and test (one test per function, or a table entry, whichever the latest entries use).
- **FR-005**: The function name and string literal appear verbatim as given, with no substitute names or tokens.
- **FR-006**: No other changes are made to the greeting package or elsewhere in this repo, and the alpha repo `handarbeit/fabrik-test-gamma` is not touched.

### Key Entities *(if applicable)*

- **`HelloE2E202611161548245157`**: A sentinel greeting function in `pkg/greeting` whose return value is the per-run-unique fixture token for this scenario run.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: `go build ./...` and `go test ./...` both pass in this repo with the change applied.
- **SC-002**: The new test passes and asserts the exact token `e2e-cross-repo-spawn-20261116-154824-5157`.
- **SC-003**: A text search of this repo's default branch after merge finds both `HelloE2E202611161548245157` and `e2e-cross-repo-spawn-20261116-154824-5157`.
- **SC-004**: The change is merged into the default branch (`main`), so the alpha repo can import the function.

## Assumptions

- The most recent existing `HelloE2E…` function and test in `pkg/greeting` define the conventions to follow. The implementer reads them to choose between one test per function and a table entry.
- The existing package conventions (package name, file layout) are sufficient, and no new files are needed.

## Out of Scope *(optional)*

- Any change to the alpha repo `handarbeit/fabrik-test-gamma`; its change is tracked separately and depends on this one.
- Any other change to the greeting package, including edits, removals or refactors of existing functions and tests.
- Any change elsewhere in this repo.

## Source References *(optional)*

- Parent issue: `handarbeit/fabrik-test-gamma#2096` (multi-issue decomposition)
- Related scenario references: #803 (on-demand spawn-target repo init), #1308 (per-run-unique fixture token)
