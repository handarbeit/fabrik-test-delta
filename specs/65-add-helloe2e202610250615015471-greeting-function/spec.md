# Feature Specification: Add HelloE2E202610250615015471 greeting function for cross-repo spawn e2e

**Feature Branch**: `fabrik/issue-65`
**Created**: 2026-10-25
**Status**: Specified
**Input**: User description: "Add a new exported greeting function to this repo's `pkg/greeting` package, together with a test. The function lets a downstream repo (`handarbeit/fabrik-test-gamma`) call it. This is the first half of a cross-repo e2e regression scenario and must land on `main` before the downstream repo can import it. Requirements: (FR-001) In `pkg/greeting/greeting.go`, add an exported function named exactly `HelloE2E202610250615015471` that takes no arguments and returns a `string`. (FR-002) The function MUST return exactly the string literal `e2e-cross-repo-spawn-20261025-061501-5471`. (FR-003) In `pkg/greeting/greeting_test.go`, add a test for this function that asserts the exact literal above. The test MUST pass with `go test ./...`. (FR-004) Use the function name and string literal verbatim. Do not shorten them, reformat them or substitute a different token. A per-run-unique token is already baked into both, and a later assertion checks for this exact text in this repo's default branch. Implementation notes: Follow the pattern of the existing `HelloE2E…` functions in `pkg/greeting/greeting.go`. Each has a doc comment followed by a one-line function that returns its sentinel string. Append the new function after the existing ones. Add the test in the same style as the existing tests in `greeting_test.go`. Do not change any other function, file or dependency in this repo. Out of scope: any other greeting function, or any other change in this repo; changes in any other repo. The downstream consumer is tracked separately and is blocked on this issue. (Spawned by Fabrik from parent issue handarbeit/fabrik-test-gamma#788 as a multi-issue decomposition.)"

## Background

This issue is the first half of a cross-repo end-to-end regression scenario. A parent issue in `handarbeit/fabrik-test-gamma` (#788) was decomposed into work in this repo and work in the downstream repo. The downstream repo needs to call a new greeting function from this repo's `pkg/greeting` package. That function must therefore exist on this repo's default branch (`main`) before the downstream work can import it.

The function name and its returned string both embed a per-run-unique token. A later assertion in the scenario looks for this exact text on this repo's default branch. Any deviation in either value, even a cosmetic one, breaks the scenario.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Downstream repo can call the new sentinel greeting (Priority: P1)

A developer or automated process in `handarbeit/fabrik-test-gamma` imports this repo's `pkg/greeting` package and calls `HelloE2E202610250615015471()`. They receive the agreed sentinel string.

**Why this priority**: It is the whole purpose of the issue. The downstream work is blocked until it lands.

**Independent Test**: Run `go test ./...` in this repo. The new test calls the function and asserts the exact literal. Separately, confirm that `HelloE2E202610250615015471` and the literal appear verbatim on `main` after merge.

**Acceptance Scenarios**:

1. **Given** the change is merged to `main`, **When** a caller invokes `HelloE2E202610250615015471()`, **Then** it returns exactly `e2e-cross-repo-spawn-20261025-061501-5471`.
2. **Given** the repo at the PR head, **When** `go build ./...` and `go test ./...` run, **Then** both succeed and the new test is among the passing tests.
3. **Given** the change is merged, **When** the e2e scenario searches this repo's default branch for the function name and the literal, **Then** both are found verbatim.

---

### Edge Cases

- A transcription error in the name or literal (a dropped digit, changed case, extra whitespace, a different separator) would pass this repo's own tests if the test copied the same error, but would break the downstream assertion. The name and literal must match this spec character for character.
- The existing `HelloE2E…` functions and their tests must be unaffected, so earlier sentinels keep working.
- The change must not alter dependencies, modules or any file other than the greeting source and its test.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: `pkg/greeting/greeting.go` exports a function named exactly `HelloE2E202610250615015471` that takes no arguments and returns a `string`.
- **FR-002**: `HelloE2E202610250615015471` returns exactly the string `e2e-cross-repo-spawn-20261025-061501-5471`, with no surrounding whitespace, prefix or suffix.
- **FR-003**: `pkg/greeting/greeting_test.go` contains a test for `HelloE2E202610250615015471` that asserts the exact literal from FR-002, and the test passes under `go test ./...`.
- **FR-004**: The function name and the string literal are used verbatim everywhere they appear: not shortened, reformatted or substituted.
- **FR-005**: The new function is added after the existing greeting functions and follows their established form (a doc comment, then a one-line function returning its sentinel). The new test follows the style of the existing tests.
- **FR-006**: No other function, file or dependency in this repo is changed.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: `go build ./...` and `go test ./...` both succeed, including the new test.
- **SC-002**: A text search of this repo's default branch finds `HelloE2E202610250615015471` and `e2e-cross-repo-spawn-20261025-061501-5471` verbatim after merge.
- **SC-003**: The diff touches only `pkg/greeting/greeting.go` and `pkg/greeting/greeting_test.go`, plus the spec file the engine projects for this issue.
- **SC-004**: The change is merged to `main`, so the downstream repo can import it.

## Assumptions

- `pkg/greeting` already exists with prior `HelloE2E…` functions and tests to use as the style reference, as the issue states.
- The engine-generated spec file under `specs/` is not an "other file" for FR-006; it is produced by the pipeline, not by the implementer.
- Landing on `main` goes through the normal PR and merge flow. The `fabrik:yolo` label means no manual approval is expected.

## Out of Scope *(optional)*

- Any other greeting function, or any other change in this repo.
- Changes in any other repo. The downstream consumer is tracked separately and is blocked on this issue.

## Source References *(optional)*

- Parent issue: `handarbeit/fabrik-test-gamma#788` (multi-issue decomposition that spawned this issue)
