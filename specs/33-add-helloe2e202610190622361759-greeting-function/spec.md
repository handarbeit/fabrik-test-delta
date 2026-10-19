# Feature Specification: Add HelloE2E202610190622361759 greeting function and test

**Feature Branch**: `fabrik/issue-33`
**Created**: 2026-10-19
**Status**: Specified
**Input**: User description: "The consumer repo `handarbeit/fabrik-test-gamma` needs to import a new exported greeting function from this repo. The function does not exist yet, so this repo has to provide it first. This is the provider half of an end-to-end regression scenario for cross-repo decomposition (#803 on-demand spawn-target repo init, #1308 per-run-unique fixture token). Export a function `HelloE2E202610190622361759() string` from the greeting package, in `pkg/greeting/greeting.go`. It MUST return exactly the string literal `e2e-cross-repo-spawn-20261019-062236-1759`. Add a test for it in `pkg/greeting/greeting_test.go` that asserts the returned literal. Use the function name and string literal verbatim; do not substitute another name or token. Note that the name's digits are `2026 10 19 06 22 36 1759` and the literal's are `20261019-062236-1759`. Copy both exactly. Append the new function at the end of `greeting.go` and `TestHelloE2E202610190622361759` at the end of `greeting_test.go`, in the same style as the earlier sentinels. Check that the current tail of both files still follows this pattern before editing; other e2e runs append to the same tail, so a conflict is possible. If one occurs, keep both sides. Do not change any other function, test or file. Acceptance: `go build ./...` and `go test ./...` pass, including the new test; after merge, beta's default branch contains `HelloE2E202610190622361759` and the literal `e2e-cross-repo-spawn-20261019-062236-1759`. Out of scope: any change to the greeting package beyond the one new function and its test; changes to Fabrik's engine or spawn logic; cleanup of fixtures from earlier runs."

## Background

The consumer repo `handarbeit/fabrik-test-gamma` needs to import a new exported greeting function from this repo. That function does not exist yet, so this repo must provide it first. This issue is the provider half of an end-to-end regression scenario for cross-repo decomposition. The scenario exercises on-demand initialisation of a spawn-target repo (#803) and a per-run-unique fixture token (#1308). The token is already baked into both the function name and the string it returns.

The greeting package already holds a series of earlier sentinel functions. Each one returns a run-specific string, and each has its own one-assertion test. This change adds the next one in that series.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Consumer repo can import the new sentinel function (Priority: P1)

A maintainer of the consumer repo (`fabrik-test-gamma`) needs to call `HelloE2E202610190622361759` from the greeting package here. Once this change is merged, that import resolves and the function returns the run's expected literal.

**Why this priority**: The consumer half of the regression scenario cannot proceed until this symbol exists on this repo's default branch. This is the only story.

**Independent Test**: Run the greeting package's tests. The new test passes, and calling the function returns exactly `e2e-cross-repo-spawn-20261019-062236-1759`.

**Acceptance Scenarios**:

1. **Given** the greeting package, **When** `HelloE2E202610190622361759()` is called, **Then** it returns exactly `e2e-cross-repo-spawn-20261019-062236-1759`.
2. **Given** the change is applied, **When** the module is built and its tests run, **Then** both succeed, including `TestHelloE2E202610190622361759`.
3. **Given** the change is merged, **When** the default branch is inspected, **Then** it contains both the identifier `HelloE2E202610190622361759` and the literal `e2e-cross-repo-spawn-20261019-062236-1759`.

---

### Edge Cases

- **Concurrent appends**: other e2e runs append sentinels to the same end of the same two files. The tails must be re-checked before editing. On a conflict, both sides are kept, so no earlier sentinel or test is dropped.
- **Digit transposition**: the name's digits (`2026 10 19 06 22 36 1759`) and the literal's digits (`20261019-062236-1759`) are laid out differently. Both must be copied exactly as written, since the consumer matches them literally.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: The greeting package exports a function `HelloE2E202610190622361759() string`, defined in `pkg/greeting/greeting.go`.
- **FR-002**: `HelloE2E202610190622361759()` returns exactly the string literal `e2e-cross-repo-spawn-20261019-062236-1759`.
- **FR-003**: `pkg/greeting/greeting_test.go` contains `TestHelloE2E202610190622361759`, which asserts that the function returns that literal.
- **FR-004**: The function name and the literal are used verbatim, with no substitute name or token.
- **FR-005**: The new function is appended at the end of `greeting.go`, and the new test at the end of `greeting_test.go`. Each follows the style of the existing sentinels: a doc comment, a one-line return, and a single-assertion test.
- **FR-006**: No other function, test or file is changed.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: `go build ./...` and `go test ./...` both pass, with the new test included in the run.
- **SC-002**: After merge, a text search of the default branch finds the identifier `HelloE2E202610190622361759` and the literal `e2e-cross-repo-spawn-20261019-062236-1759`.
- **SC-003**: The change touches only the greeting package's source file and test file, with only additions and no edits to existing sentinels.

## Assumptions

- The tails of both files still follow the sentinel pattern, which was confirmed when this spec was written. Any later conflict from concurrent e2e runs is resolved by keeping both sides.
- The per-run token in the name and literal is correct as given and is not to be second-guessed or regenerated.

## Out of Scope

- Any change to the greeting package beyond the one new function and its test.
- Changes to Fabrik's engine or spawn logic.
- Cleanup of fixtures from earlier runs.

## Source References

- Parent issue: `handarbeit/fabrik-test-gamma#418`, the multi-issue decomposition that spawned this issue (the plan is linked from there).
- Fabrik #803 (on-demand spawn-target repo init) and #1308 (per-run-unique fixture token), which this scenario regression-tests.
