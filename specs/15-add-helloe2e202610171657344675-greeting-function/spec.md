# Feature Specification: Add per-run-unique greeting function `HelloE2E202610171657344675`

**Feature Branch**: `fabrik/issue-15`
**Created**: 2026-10-17
**Status**: Specified
**Input**: User description:

> # Feature: add a per-run-unique greeting function
>
> ## Problem
>
> The `pkg/greeting` package in this repository needs a new exported function that returns a fixed, per-run-unique string. A downstream consumer repo (`handarbeit/fabrik-test-gamma`) will import and call it, so it must exist on this repository's default branch first. This is a fixture for an end-to-end regression test of cross-repo spawning.
>
> ## Requirements
>
> - **R1.** In `pkg/greeting/greeting.go`, add an exported function with this exact name and signature:
>   `func HelloE2E202610171657344675() string`
> - **R2.** The function MUST return exactly this string literal, with no surrounding whitespace and no other characters:
>   `e2e-cross-repo-spawn-20261017-165734-4675`
> - **R3.** Give the function a one-line doc comment, matching the style of the other `HelloE2E…` functions in the same file.
> - **R4.** In `pkg/greeting/greeting_test.go`, add a test named `TestHelloE2E202610171657344675`. It must assert that `HelloE2E202610171657344675()` returns exactly `e2e-cross-repo-spawn-20261017-165734-4675`. Follow the existing pattern in that file: a `const want` and a `got != want` check that calls `t.Errorf` or `t.Fatalf` on mismatch.
> - **R5.** If `pkg/greeting/greeting.go` or `pkg/greeting/greeting_test.go` does not exist, create it with the correct `package greeting` declaration and the code above. Otherwise append to the existing file. Do not modify or remove any existing function or test.
> - **R6.** The function name and string literal are the source of truth. Use them verbatim. Do not regenerate, normalise, re-case or invent a different token. A different token fails the scenario's final assertion.
>
> ## Scope
>
> Only the single function and its test. No other behaviour changes, refactors or dependency changes.
>
> ## Acceptance
>
> - `pkg/greeting/greeting.go` on the default branch exports `HelloE2E202610171657344675() string` returning exactly `e2e-cross-repo-spawn-20261017-165734-4675`.
> - `pkg/greeting/greeting_test.go` contains `TestHelloE2E202610171657344675`, and `go test ./...` passes in this repository.
> - The text `HelloE2E202610171657344675` and the string `e2e-cross-repo-spawn-20261017-165734-4675` are present on the default branch once this issue's PR is merged.
>
> ## Notes
>
> Mentioned for context: the consumer repo `handarbeit/fabrik-test-gamma` will bump its dependency pin to a commit containing this function, and will call it from its `main.go`, after this issue is merged. That work is tracked separately and is not part of this issue's acceptance criteria.
>
> ---
>
> *Spawned by Fabrik from parent issue handarbeit/fabrik-test-gamma#249 as a multi-issue decomposition. The parent's plan is at the link above.*

## Background

The `pkg/greeting` package in this repository is a fixture for end-to-end regression tests of Fabrik's cross-repo spawning. A downstream consumer repository (`handarbeit/fabrik-test-gamma`, parent issue #249) must import and call a new sentinel function, so the function has to exist on this repository's default branch (`main`) before the consumer can bump its dependency pin. The scenario's final assertion looks for the exact function name and the exact returned string, so the token is a fixed, externally supplied value and must be used verbatim.

The package already follows an established convention. Each earlier run added one exported `HelloE2E<timestamp>` function that returns a fixed `e2e-cross-repo-spawn-…` string. Each function has a one-line doc comment and a matching table-free test that declares `const want` and compares it to the returned value. This feature adds one more instance of that pattern.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Consumer repo can call the new sentinel (Priority: P1)

A maintainer of the downstream consumer repo (`fabrik-test-gamma`) needs to import this repository's `greeting` package and call `HelloE2E202610171657344675()`, expecting the fixed per-run-unique string.

**Why this priority**: This is the entire purpose of the issue. The cross-repo spawn scenario fails without it.

**Independent Test**: Run the package's tests, or call the function from a throwaway program, and compare the result to the expected literal.

**Acceptance Scenarios**:

1. **Given** the change is merged to `main`, **When** a consumer calls `HelloE2E202610171657344675()`, **Then** it receives exactly `e2e-cross-repo-spawn-20261017-165734-4675`, with no surrounding whitespace or other characters.
2. **Given** the change is merged to `main`, **When** the repository's full test suite runs, **Then** it passes and includes `TestHelloE2E202610171657344675`.

---

### Edge Cases

- A near-identical sentinel already exists (for example `HelloE2E202610171410524438`). The new function must be distinct and must not alter or replace any existing function or test.
- The token is easy to mistype (digit transposition, re-casing, or a different timestamp). Any deviation from the supplied name or literal fails the downstream assertion, so the exact values are used without normalisation.
- The target files may not exist in some checkouts. In that case they are created with the correct `package greeting` declaration. In this repository both files currently exist, so the new code is appended.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: The `greeting` package MUST export a function named exactly `HelloE2E202610171657344675` with signature `func() string`.
- **FR-002**: The function MUST return exactly `e2e-cross-repo-spawn-20261017-165734-4675`, with no surrounding whitespace and no other characters.
- **FR-003**: The function MUST have a one-line doc comment in the style of the existing `HelloE2E…` functions.
- **FR-004**: The package's tests MUST include a test named `TestHelloE2E202610171657344675` that asserts the function returns exactly `e2e-cross-repo-spawn-20261017-165734-4675`. The test MUST follow the existing pattern: a `const want`, then a `got != want` check that reports a mismatch via `t.Errorf` or `t.Fatalf`.
- **FR-005**: The change MUST be purely additive. No existing function or test may be modified or removed, and the new code is appended to the existing source and test files. If either file is missing, it is created with a correct `package greeting` declaration.
- **FR-006**: The function name and string literal MUST be used verbatim as supplied. They must not be regenerated, normalised, re-cased or replaced.
- **FR-007**: The change MUST NOT introduce any other behaviour change, refactor or dependency change. The spec copy that the engine manages under `specs/` is exempt from this restriction.

### Key Entities

- **Sentinel function**: A zero-argument exported function in the `greeting` package that returns a fixed, per-run-unique string. Downstream repositories use it to prove that a cross-repo change landed.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: On `main` after merge, calling `HelloE2E202610171657344675()` returns a string byte-for-byte equal to `e2e-cross-repo-spawn-20261017-165734-4675`.
- **SC-002**: `go test ./...` passes in this repository, and the run includes `TestHelloE2E202610171657344675`.
- **SC-003**: After merge, the text `HelloE2E202610171657344675` and the string `e2e-cross-repo-spawn-20261017-165734-4675` are both present on the default branch. Apart from the engine-managed spec copy, they appear only in the new function and its test.
- **SC-004**: Every function and test that existed before the change is still present and unmodified.

## Assumptions

- The default base branch is `main`, and the existing `greeting` source and test files are the places to append to.
- The existing `HelloE2E…` functions' doc-comment style is the "returns the sentinel string for the cross-repo spawn regression test." form. Following it satisfies the "one-line doc comment" requirement.
- Updating the downstream consumer (dependency pin and `main.go` call) is handled separately in `fabrik-test-gamma`.

## Out of Scope *(optional)*

- Any change in `handarbeit/fabrik-test-gamma`, including bumping its dependency pin or calling the function from its `main.go`.
- Changes to existing greeting functions, tests, documentation or dependencies.
- Refactoring or consolidating the accumulating `HelloE2E…` sentinels.
