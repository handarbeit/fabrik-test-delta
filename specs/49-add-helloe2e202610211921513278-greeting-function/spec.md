# Feature Specification: Add `HelloE2E202610211921513278` to the greeting package

**Feature Branch**: `fabrik/issue-49`
**Created**: 2026-10-21
**Status**: Specified
**Input**: User description: "Add `HelloE2E202610211921513278` greeting function and test (cross-repo spawn e2e). This is the library half of a cross-repo e2e regression scenario (#803 on-demand spawn-target repo init, #1308 per-run-unique fixture token). A consumer repo (`handarbeit/fabrik-test-gamma`) will import the new function once this change has landed, so this change must merge first. R1: Add an exported function `HelloE2E202610211921513278() string` to `pkg/greeting/greeting.go`. It MUST return exactly the string literal `e2e-cross-repo-spawn-20261021-192151-3278`. R2: Add a test for it in `pkg/greeting/greeting_test.go`. The test must assert the exact return value, in the same form as the existing sentinel tests (a `const want = \"e2e-cross-repo-spawn-20261021-192151-3278\"` and an `if got := HelloE2E202610211921513278(); got != want { t.Errorf(...) }` check), named `TestHelloE2E202610211921513278`. R3: Follow the style of the existing `HelloE2E…` sentinel functions. Append the new function at the end of `greeting.go` with a doc comment of the form `// HelloE2E202610211921513278 returns the sentinel string for the cross-repo spawn regression test.`, and append the new test at the end of `greeting_test.go`. R4: The function name and the string literal MUST be used verbatim. Do not normalise, shorten, regenerate or substitute the date/time or random token, because the scenario's final assertion depends on this exact text landing on this repo's default branch."

## Background

This is the library half of a cross-repo end-to-end regression scenario. The scenario covers on-demand initialisation of a spawn-target repository (#803) and per-run-unique fixture tokens (#1308). A consumer repo, `handarbeit/fabrik-test-gamma`, will import the new function once this change has landed, so this change must merge first. The scenario's final assertion depends on the exact function name and string literal appearing on this repo's default branch, which is why both must be used verbatim.

The greeting package already contains other `HelloE2E…` sentinel functions with matching tests. This change adds one more in the same style.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Sentinel function available to the consumer repo (Priority: P1)

The consumer repo's e2e scenario imports `HelloE2E202610211921513278` from the greeting package and asserts it returns the per-run-unique token. The function must therefore exist on this repo's default branch with the exact name and return value.

**Why this priority**: It is the whole purpose of the change. Without it the cross-repo scenario cannot pass.

**Independent Test**: Call `HelloE2E202610211921513278()` and compare the result to `e2e-cross-repo-spawn-20261021-192151-3278`.

**Acceptance Scenarios**:

1. **Given** the change is merged to the default branch, **When** a caller invokes `HelloE2E202610211921513278()`, **Then** it returns exactly `e2e-cross-repo-spawn-20261021-192151-3278`.
2. **Given** the change is merged to the default branch, **When** `go test ./...` runs in this repo, **Then** all tests pass, including `TestHelloE2E202610211921513278`.

---

### Edge Cases

- The name or literal is normalised, shortened, regenerated or has its date/time or random token substituted: the consumer scenario's final assertion fails, so any such change is a defect.
- An existing function, test or other file is modified, removed or reordered: this violates the purely additive requirement.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: `pkg/greeting/greeting.go` MUST export a function `HelloE2E202610211921513278() string` that returns exactly the string literal `e2e-cross-repo-spawn-20261021-192151-3278`.
- **FR-002**: `pkg/greeting/greeting_test.go` MUST contain a test named `TestHelloE2E202610211921513278` that asserts the exact return value, in the same form as the existing sentinel tests (a `const want = "e2e-cross-repo-spawn-20261021-192151-3278"` and an `if got := HelloE2E202610211921513278(); got != want { t.Errorf(...) }` check).
- **FR-003**: The new function MUST be appended at the end of `greeting.go`, with the doc comment `// HelloE2E202610211921513278 returns the sentinel string for the cross-repo spawn regression test.`. The new test MUST be appended at the end of `greeting_test.go`. Both follow the style of the existing `HelloE2E…` sentinels.
- **FR-004**: The function name and the string literal MUST be used verbatim, with no normalisation, shortening, regeneration or substitution of the date/time or random token.
- **FR-005**: The change MUST be purely additive: no existing function, test or other file is modified, removed or reordered.
- **FR-006**: The PR for this issue MUST close this issue with `Closes #49`.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: On the default branch, `pkg/greeting/greeting.go` contains `HelloE2E202610211921513278` returning `e2e-cross-repo-spawn-20261021-192151-3278`.
- **SC-002**: `TestHelloE2E202610211921513278` exists and passes, and `go test ./...` passes in this repo.
- **SC-003**: The diff against the base branch contains only additions to `greeting.go` and `greeting_test.go` (plus the engine-written spec file), with no modified or removed existing lines.

## Assumptions

- The existing sentinel functions and tests follow a consistent shape that the new ones can mirror.
- Consumers resolve this repo through pseudo-versions, so no tag or release is needed.

## Out of Scope *(optional)*

- Changes to any other function in the greeting package.
- Tagging or releasing this repo.
- Any change to the consumer repo (`handarbeit/fabrik-test-gamma`).

## Acceptance

- `pkg/greeting/greeting.go` on the default branch contains `HelloE2E202610211921513278` returning `e2e-cross-repo-spawn-20261021-192151-3278`.
- The new test exists and passes, and `go test ./...` passes in this repo.
- The change is purely additive: no existing function, test or other file is modified, removed or reordered.

## Notes

- The PR for this issue must close this issue with `Closes #<this issue number>`.
---

*Spawned by Fabrik from parent issue handarbeit/fabrik-test-gamma#608 as a multi-issue decomposition. The parent's plan is at the link above.*
