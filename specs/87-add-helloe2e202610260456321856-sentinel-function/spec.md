# Feature Specification: Add HelloE2E202610260456321856 sentinel function to pkg/greeting

**Feature Branch**: `fabrik/issue-87`
**Created**: 2026-10-26
**Status**: Specified
**Input**: User description: "## Problem

The consumer repo `handarbeit/fabrik-test-gamma` needs to call a new sentinel greeting function from this repo's `pkg/greeting` package. The function does not exist yet, so the consumer cannot compile against it. This repo's change must merge to `main` first.

This is part of an end-to-end regression scenario for cross-repo decomposition (on-demand spawn-target init, and a per-run-unique fixture token). The function name and string literal below already contain this run's unique token. **Use them exactly as written. Do not rename, regenerate or alter them.** The scenario's final assertion checks for this exact text in this repo.

## Requirements

1. In `pkg/greeting/greeting.go`, add an exported function with no parameters that returns a string:

   - Name: `HelloE2E202610260456321856`
   - Signature: `func HelloE2E202610260456321856() string`
   - Return value: exactly the literal `e2e-cross-repo-spawn-20261026-045632-1856`

   Follow the style of the existing sentinel functions in the same file (for example `HelloE2E202610251306590520`): add a doc comment of the form `// HelloE2E202610260456321856 returns the sentinel string for the cross-repo spawn regression test.` and place the new function after the existing sentinel functions.

2. In `pkg/greeting/greeting_test.go`, add a test named `TestHelloE2E202610260456321856`. Follow the existing pattern: a `const want = \"e2e-cross-repo-spawn-20261026-045632-1856\"`, call the function, and `t.Errorf` if `got != want`.

## Scope

- Add only the one function and its one test.
- Do not modify, rename or remove any existing function or test.
- Do not introduce any other token, name or literal.
- Do not change any other file, except any spec file the pipeline itself generates.

## Acceptance

- `go build ./...` and `go test ./...` pass in this repo.
- `HelloE2E202610260456321856()` returns exactly `e2e-cross-repo-spawn-20261026-045632-1856`.
- `TestHelloE2E202610260456321856` exists and passes.
- The change is merged to this repo's default branch (`main`)."

## Background

The consumer repo `handarbeit/fabrik-test-gamma` (parent issue handarbeit/fabrik-test-gamma#1063) needs to call a new sentinel greeting function from this repo's `pkg/greeting` package. The function does not exist yet, so the consumer cannot compile against it until this change has merged to `main`.

This is one part of an end-to-end regression scenario for cross-repo decomposition (on-demand spawn-target init, and a per-run-unique fixture token). The function name and string literal contain this run's unique token. The scenario's final assertion looks for this exact text in this repo, so the token must be used exactly as given. The package already contains several earlier sentinel functions of the same kind, such as `HelloE2E202610251306590520`, and this work follows their established style.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Consumer can call the new sentinel function (Priority: P1)

A developer or automated scenario in `handarbeit/fabrik-test-gamma` needs to import this repo's `pkg/greeting` package and call `HelloE2E202610260456321856()`. Once this change is on `main`, that call compiles and returns the expected sentinel string.

**Why this priority**: It is the only deliverable of the issue. The consumer repo cannot build until it exists on `main`.

**Independent Test**: Run the new unit test, then `go build ./...` and `go test ./...` in this repo. The function's return value can also be checked directly.

**Acceptance Scenarios**:

1. **Given** the change is merged to `main`, **When** a consumer calls `HelloE2E202610260456321856()`, **Then** it returns exactly `e2e-cross-repo-spawn-20261026-045632-1856`.
2. **Given** the repo at the change's head, **When** `go build ./...` and `go test ./...` are run, **Then** both succeed.
3. **Given** the repo at the change's head, **When** the tests are listed, **Then** `TestHelloE2E202610260456321856` exists and passes.

---

### Edge Cases

- A token that is renamed, regenerated or altered (name or literal) would make the scenario's final assertion fail, so the exact text must be preserved.
- Existing sentinel functions and tests must remain unchanged, so no regression is introduced for other scenarios that depend on them.
- Any additional name or literal not in the issue would violate the scope, so none may be added.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: The `pkg/greeting` package exports a function `HelloE2E202610260456321856` that takes no parameters and returns a `string`.
- **FR-002**: `HelloE2E202610260456321856()` returns exactly the literal `e2e-cross-repo-spawn-20261026-045632-1856`.
- **FR-003**: The function is documented with the doc comment `// HelloE2E202610260456321856 returns the sentinel string for the cross-repo spawn regression test.` and is placed after the existing sentinel functions in `pkg/greeting/greeting.go`.
- **FR-004**: A test named `TestHelloE2E202610260456321856` exists in `pkg/greeting/greeting_test.go`. Following the existing pattern, it compares the function's result to a `want` constant of `e2e-cross-repo-spawn-20261026-045632-1856` and reports an error if they differ.
- **FR-005**: No existing function or test is modified, renamed or removed, and no other token, name or literal is introduced.
- **FR-006**: No file other than `pkg/greeting/greeting.go`, `pkg/greeting/greeting_test.go` and the spec file the pipeline generates is changed.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: `go build ./...` and `go test ./...` both pass in this repo with the change applied.
- **SC-002**: The exact text `HelloE2E202610260456321856` and `e2e-cross-repo-spawn-20261026-045632-1856` appears in this repo, as the scenario's final assertion requires.
- **SC-003**: The change is merged to `main`, so the consumer repo can build against it.

## Assumptions

- The existing sentinel functions in `pkg/greeting/greeting.go` and their tests are the style reference, and this change copies them exactly.
- "After the existing sentinel functions" means after the last sentinel function currently in the file.
- The new function and test are the only changes needed to satisfy the consumer repo.

## Out of Scope

- Any change to the consumer repo `handarbeit/fabrik-test-gamma`.
- Modifying, renaming, removing or refactoring existing sentinel functions or tests.
- Any other file changes beyond the function, its test and the pipeline-generated spec.
