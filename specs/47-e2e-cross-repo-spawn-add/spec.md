# Feature Specification: Add HelloE2E202610192129182382 to pkg/greeting

**Feature Branch**: `fabrik/issue-47`
**Created**: 2026-10-19
**Status**: Specified
**Input**: User description: "## Goal

Add one exported greeting function, with a test, to the `pkg/greeting` package of this repo (`handarbeit/fabrik-test-delta`). A dependent repo (`handarbeit/fabrik-test-gamma`) will call it once this change has merged, so this work must be complete and merged to the default branch on its own.

## Requirements

1. In `pkg/greeting/greeting.go`, add an exported function named exactly `HelloE2E202610192129182382` with the signature `func HelloE2E202610192129182382() string`.
   - It must return exactly the string literal `\"e2e-cross-repo-spawn-20261019-212918-2382\"`.
   - Add it after the existing `HelloE2E…` functions and follow their form: a doc comment, then a one-line function body returning the literal.
2. In `pkg/greeting/greeting_test.go`, add a top-level test `TestHelloE2E202610192129182382`.
   - Follow the form of the existing `TestHelloE2E…` tests: a `const want = \"e2e-cross-repo-spawn-20261019-212918-2382\"` and a `got != want` check that calls `t.Errorf` or `t.Fatalf`.
3. Use the function name and the string literal verbatim, character for character.
   - A per-run-unique token is baked into them, and a regression test later checks for that exact text in this repo.
   - Do not shorten, reuse, vary or regenerate them.

## Scope

- This change is purely additive. Do not modify, rename or remove any existing function or test.
- Do not add other greeting functionality.
- No documentation, CLI, config or dependency changes are needed.

## Acceptance Criteria

- `HelloE2E202610192129182382()` exists in `pkg/greeting/greeting.go` and returns exactly `e2e-cross-repo-spawn-20261019-212918-2382`.
- `TestHelloE2E202610192129182382` exists in `pkg/greeting/greeting_test.go`, asserts that exact string and passes.
- `go build ./...`, `go vet ./...` and `go test ./...` pass.
- The change is merged to the default branch.

Mention nothing about other repos' branches or unmerged work as a requirement. This issue stands alone on this repo's default branch.

*Spawned by Fabrik from parent issue handarbeit/fabrik-test-gamma#595 as a multi-issue decomposition. The parent's plan is at the link above.*"

## Background

A dependent repository (`handarbeit/fabrik-test-gamma`) needs to call a new greeting function from `pkg/greeting` in this repo. That is only possible once the function exists on this repo's default branch (`main`), so the work must be complete and mergeable on its own, with no reliance on any other repo's branches or unmerged work. The function name and returned string carry a per-run-unique token (`20261019-212918-2382`). A later regression test checks for that exact text in this repo, so it must be reproduced character for character. The package already holds many sentinel functions and tests of the same `HelloE2E…` form (a doc comment and a one-line body returning a literal), so this change follows that established pattern.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Consumers can call the new sentinel greeting (Priority: P1)

A maintainer of the dependent repo wants to call `HelloE2E202610192129182382()` from `pkg/greeting` and get the agreed, run-unique string back. The function exists on this repo's default branch, with a passing test guarding its output.

**Why this priority**: This is the whole deliverable. The dependent repo's work is blocked until it exists on `main`.

**Independent Test**: Run `go test ./pkg/greeting/...` and confirm `TestHelloE2E202610192129182382` exists and passes. Check that the function returns exactly `e2e-cross-repo-spawn-20261019-212918-2382`.

**Acceptance Scenarios**:

1. **Given** the change is merged to `main`, **When** a caller invokes `HelloE2E202610192129182382()`, **Then** it returns exactly `e2e-cross-repo-spawn-20261019-212918-2382`.
2. **Given** the change is applied, **When** `go build ./...`, `go vet ./...` and `go test ./...` are run, **Then** all three succeed.
3. **Given** the change is applied, **When** the existing `HelloE2E…` functions and tests are inspected, **Then** none of them has been modified, renamed or removed.

---

### Edge Cases

- The run-unique token must not be shortened, reused from another sentinel, varied or regenerated. Any deviation breaks the later regression check.
- The new function and test must be additive only. An edit to an existing sentinel, even a formatting one, falls outside scope.
- The change must not depend on, or mention as a requirement, any branch or unmerged work in other repos.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: `pkg/greeting` MUST export a function named exactly `HelloE2E202610192129182382` with the signature `func HelloE2E202610192129182382() string`.
- **FR-002**: `HelloE2E202610192129182382` MUST return exactly the string `e2e-cross-repo-spawn-20261019-212918-2382`.
- **FR-003**: The function MUST be placed after the existing `HelloE2E…` functions and follow their form: a doc comment, then a one-line body returning the literal.
- **FR-004**: The package's test file MUST contain a top-level test named exactly `TestHelloE2E202610192129182382`, following the form of the existing `TestHelloE2E…` tests. It uses a `const want = "e2e-cross-repo-spawn-20261019-212918-2382"` and a `got != want` check that reports failure via `t.Errorf` or `t.Fatalf`.
- **FR-005**: The function name and string literal MUST appear verbatim, character for character, with no variation.
- **FR-006**: The change MUST be purely additive: no existing function or test is modified, renamed or removed, and no other greeting functionality is added.
- **FR-007**: The change MUST be mergeable to this repo's default branch (`main`) on its own, with no dependency on other repos' branches or unmerged work.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: After merge, `HelloE2E202610192129182382()` returns exactly `e2e-cross-repo-spawn-20261019-212918-2382` on `main`.
- **SC-002**: `TestHelloE2E202610192129182382` exists and passes.
- **SC-003**: `go build ./...`, `go vet ./...` and `go test ./...` all pass with no new failures.
- **SC-004**: The diff against the base contains only additions to `pkg/greeting/greeting.go` and `pkg/greeting/greeting_test.go` (plus the engine-generated spec file), with no deletions or edits to existing lines.
- **SC-005**: A text search of the repo for the exact name and the exact literal finds them in the intended files.

## Assumptions

- The existing `HelloE2E…` functions and tests are the reference for form and placement. "After the existing functions" means appended at the end of the existing sentinel group in each file.
- The spec file the engine writes under `specs/` is the only non-code addition, and it does not count as a documentation change.
- No documentation, CLI, config or dependency updates are required.

## Out of Scope *(optional)*

- Any other greeting functionality, or changes to existing sentinels.
- Documentation, CLI, configuration or dependency changes.
- Work in `handarbeit/fabrik-test-gamma` or any other repo, including the call to this function.

## Source References *(optional)*

- Parent issue: handarbeit/fabrik-test-gamma#595 (multi-issue decomposition that spawned this issue)
