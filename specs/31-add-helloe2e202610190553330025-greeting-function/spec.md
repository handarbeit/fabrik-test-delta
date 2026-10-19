# Feature Specification: Add HelloE2E202610190553330025 greeting function for e2e cross-repo spawn

**Feature Branch**: `fabrik/issue-31`
**Created**: 2026-10-19
**Status**: Specified
**Input**: User description: "## Goal

Add one exported function and a test to the `pkg/greeting` package so a downstream repo (`handarbeit/fabrik-test-gamma`) can import it. This is part of an end-to-end regression scenario for cross-repo decomposition (#803 on-demand spawn-target repo init, #1308 per-run-unique fixture token).

## Requirements

1. In `pkg/greeting/greeting.go`, add the exported function `HelloE2E202610190553330025() string`. It must return exactly the string `e2e-cross-repo-spawn-20261019-055333-0025`. Follow the existing style in the file: one exported function per run token, with a doc comment of the form `// HelloE2E202610190553330025 returns the sentinel string for the cross-repo spawn regression test.`
2. In `pkg/greeting/greeting_test.go`, add `TestHelloE2E202610190553330025`. It must assert that `HelloE2E202610190553330025()` returns exactly `e2e-cross-repo-spawn-20261019-055333-0025`. Follow the existing test shape: a `const want` and a `got != want` check that calls `t.Errorf`.

## Constraints

- Use the function name `HelloE2E202610190553330025` and the string literal `e2e-cross-repo-spawn-20261019-055333-0025` verbatim. Do not rename, alter or regenerate the token. It is unique to this run, and the scenario's final assertion checks this exact text in this repo.
- Do not change or remove any existing function or test in the package.
- Do not change anything beyond this one function and its test.
- If `pkg/greeting/greeting.go` or `pkg/greeting/greeting_test.go` does not exist, create it with `package greeting` and the content above.

## Acceptance

- `pkg/greeting/greeting.go` on the default branch exports `HelloE2E202610190553330025() string` returning exactly `e2e-cross-repo-spawn-20261019-055333-0025`.
- `go test ./...` passes in this repo, including the new test.
- Once merged, the downstream repo bumps its pin to the merged commit, so this change must land on the default branch."

## Background

This issue was spawned by Fabrik from parent issue `handarbeit/fabrik-test-gamma#408` as part of a multi-issue decomposition. It is one leg of an end-to-end regression scenario for cross-repo decomposition: on-demand initialisation of a spawn-target repo (#803) and a per-run-unique fixture token (#1308). The `pkg/greeting` package in this repo is meant to be imported by a downstream repo (`handarbeit/fabrik-test-gamma`), which will bump its dependency pin to the merged commit. The scenario's final assertion checks that this exact token text exists on this repo's default branch, so the token must be reproduced exactly and the change must be merged.

The package already follows a convention of one exported sentinel function per run token, each with a matching test. The most recent entries are `HelloE2E202610171410524438` and `HelloE2E202610171657344675`.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Downstream repo can import the run's sentinel (Priority: P1)

A downstream repo (`handarbeit/fabrik-test-gamma`) imports `pkg/greeting` from this repo and calls `HelloE2E202610190553330025()` to obtain the sentinel string for this run.

**Why this priority**: This is the whole purpose of the issue; the end-to-end scenario cannot pass without it.

**Independent Test**: Call `HelloE2E202610190553330025()` from a test in this package and compare with the expected string.

**Acceptance Scenarios**:

1. **Given** the change is merged to the default branch, **When** a caller invokes `HelloE2E202610190553330025()`, **Then** it returns exactly `e2e-cross-repo-spawn-20261019-055333-0025`.
2. **Given** the change is merged, **When** the downstream repo bumps its pin to the merged commit, **Then** it can import and call the function.

---

### Edge Cases

- The token and string literal must be reproduced character-for-character; any alteration breaks the scenario's final assertion.
- Existing sentinel functions and tests must remain unchanged and passing.
- If either `pkg/greeting/greeting.go` or `pkg/greeting/greeting_test.go` were absent, it would be created with `package greeting` (both currently exist).

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: `pkg/greeting` exports a function `HelloE2E202610190553330025() string` that returns exactly `e2e-cross-repo-spawn-20261019-055333-0025`.
- **FR-002**: The function has a doc comment of the form `// HelloE2E202610190553330025 returns the sentinel string for the cross-repo spawn regression test.`, matching the package's existing one-function-per-run-token style.
- **FR-003**: A test named `TestHelloE2E202610190553330025` asserts that `HelloE2E202610190553330025()` returns exactly `e2e-cross-repo-spawn-20261019-055333-0025`, using a `const want` and a `got != want` check that reports with `t.Errorf`, as the existing tests do.
- **FR-004**: The function name and string literal are used verbatim and are not renamed, altered or regenerated.
- **FR-005**: No existing function or test in the package is changed or removed, and nothing is changed beyond this one function and its test.
- **FR-006**: The change lands on the repository's default branch (`main`).

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: On the default branch, `HelloE2E202610190553330025()` returns exactly `e2e-cross-repo-spawn-20261019-055333-0025`.
- **SC-002**: `go test ./...` passes in this repo, including `TestHelloE2E202610190553330025`.
- **SC-003**: The change set contains only the one new function and its one new test; all pre-existing tests in the package still pass.
- **SC-004**: The downstream repo can bump its pin to the merged commit and the scenario's final assertion finds the exact token text in this repo.

## Assumptions

- Both `pkg/greeting/greeting.go` and `pkg/greeting/greeting_test.go` already exist, so no new files are created.
- The new function and test are appended after the existing entries, following the established ordering.
- The engine-managed copy of this spec at `specs/31-*/spec.md` is not part of the "nothing beyond one function and its test" constraint, consistent with how earlier sentinel issues were handled.

## Out of Scope

- Any change to existing sentinel functions or tests.
- Refactoring, renaming or consolidating the per-run-token convention.
- Updating the downstream repo's pin (handled in `handarbeit/fabrik-test-gamma`).
- Any change outside `pkg/greeting`.
