# Engineering Principles

<!-- agent-template · core v0.1 -->

Stack-agnostic rules that apply to every project. Stack-specific rules belong in
sibling module files; project-specific commands, paths, and architecture belong
in the project's own `AGENTS.md`. Don't add either here.

## Before you call it done

- Run the project's format, lint, typecheck, and test commands. A change isn't
  done until they pass — say so plainly if they don't.
- Re-read your own diff. Delete anything you added and stopped using.
- Changed an API route? Regenerate the spec and client in the same change.
- Changed behavior the docs describe? Update the docs in the same change.

## Architecture boundaries

Layer names differ by project (handler/controller, service, repository/
transaction), the boundaries don't:

| Layer | Owns | Never contains |
| --- | --- | --- |
| Handler / Controller | HTTP in/out, parsing, validation, status codes | business logic |
| Service | business rules | HTTP types, SQL |
| Repository / Transaction | data access | business logic |

- Data flows down, errors flow up: repository → service → handler.
- Cross a boundary through an interface, not a concrete type.
- Pass dependencies explicitly through constructors. No globals, no singletons
  reached via import side effects, no hidden state.

## Errors

- One error taxonomy per project. Don't invent an ad-hoc error shape per endpoint.
- Convert infrastructure errors to domain errors at the service boundary.
- Never return a raw database error, driver error, or stack trace to a client.
  Log the full error server-side; return a safe message and the correct status.
- Translate known constraint violations into something a user can act on:
  unique violation → "Email already exists", FK violation → "Referenced
  resource not found".

## Functions and naming

- One function, one job. Prefer under ~40 lines. Extract a helper before nesting
  a third conditional.
- Name by what it does, not how: `FindUserByID`, `CalculateInvoiceTotal`.
  Reject `Handle`, `Process`, `DoThing`, `data`, `temp`.
- No abstraction for a single caller. No interface until there's a second
  implementation or a test that needs to mock it.
- The surrounding code's idiom, comment density, and naming beat any general
  preference stated here.

## No hardcoded values

Anything someone might reasonably want to change without a code review belongs
in config, constants, design tokens, or environment variables — page sizes,
timeouts, retry counts, colors, spacing, URLs, feature flags, limits.

## Dead code

- No unused imports, variables, or parameters.
- No commented-out code. Git remembers it.
- No leftover debug output — `println`, `console.log`, `.only` in tests.
- Production paths use structured logging, not print statements.

## Data access

- No unbounded lists. Every list endpoint paginates; prefer cursor/keyset over
  offset, which degrades as the table grows.
- Cap page size server-side. A client asking for 10,000 rows gets the cap.
- Return only the fields the caller needs.
- No N+1. Fetch related data in one round trip; batch instead of looping.

## Secrets and input

- Secrets come from the secret manager or the environment. Never commit one,
  never log one, never put one in an error message.
- Validate and parse every request payload at the edge, before it reaches
  business logic. Reject unknown fields on write paths.
- Parameterize every query. Never build SQL by string concatenation.

## Tests

Cover, in rough order of what actually catches bugs:

- **Happy path** — the expected flow.
- **Error paths** — invalid input, missing resource, unauthorized.
- **Edge cases** — empty, boundary values, duplicates, malformed input,
  concurrent requests.
- **Lifecycle** — create → read → update → delete → verify gone.
- **Idempotency** — calling it twice is safe.

Rules:

- Mock external services and the clock. Tests must not depend on the network,
  wall-clock time, or execution order.
- A test that asserts nothing is not a test.
- When you fix a bug, add the test that would have caught it.

## Version control

- Conventional Commits: `feat:`, `fix:`, `docs:`, `chore:`, `refactor:`, `test:`.
- Small, focused PRs. Split a large feature into reviewable pieces.
- The message explains *why*; the diff already shows *what*.
- Never hand-edit generated files — mocks, API clients, migration output.
  Change the source and regenerate.

## Refactoring

Preserve behavior unless told otherwise. Reduce complexity, improve names,
remove duplication, enforce the boundaries above. Don't expand into unrelated
files along the way.

## When you're unsure

- An existing pattern in this codebase beats the general advice here.
- If two existing patterns conflict, ask rather than silently picking one.
- State any assumption you had to make when you summarize the change.
