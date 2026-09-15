# <Project Name>

<!-- agent-template · core v0.1 -->
<!-- Fill in every section below. Delete the TODO comments as you go.
     Do not delete a section because you don't have the answer yet —
     an empty Commands block is a bug, not a style choice. -->

## Commands

<!-- TODO: the exact commands, copy-pasteable, with the task runner you use
     (just / task / make / npm). This is the single highest-value section in
     this file. If an agent has to guess how to run your tests, it will guess
     wrong. Include anything with a non-obvious flag or a known failure mode. -->

| Task | Command |
| --- | --- |
| Install deps | `` |
| Start backend | `` |
| Start frontend | `` |
| Run tests | `` |
| Lint | `` |
| Format | `` |
| Typecheck | `` |
| Create migration | `` |
| Apply migrations | `` |
| Regenerate API spec/client | `` |
| Regenerate mocks | `` |

<!-- TODO: note any command that hangs, needs a running container, or must not
     be run against production. Example:
     - `just test-be` needs the DB up first: `just up-db`
     - Never run `just migrate-*-prod` without review. -->

## Architecture

<!-- TODO: ~10 lines. Only what an agent can't infer by reading the tree.
     Where each layer lives, what the non-obvious boundaries are, and any
     place the code deviates from the principles below (and why). -->

```
<!-- TODO: directory tree, backend and frontend, one line of purpose each -->
```

## Stack

<!-- TODO: one line per major choice, enough that an agent doesn't reach for
     the wrong library. Language + framework + ORM + migration tool + auth +
     server state + styling + secret manager. -->

## Project conventions

<!-- TODO: rules specific to THIS repo that override or extend the principles
     below. Design token names, error constructors, test helpers, generated
     directories that must never be hand-edited. Delete this section if there
     genuinely aren't any yet — but there usually are. -->

---

<!-- Everything below is shared across all GenerateNU projects.
     Source: github.com/GenerateNU/agent-template
     Don't edit it here; open a PR against the template so every project
     gets the fix. Project-specific rules go in the sections above. -->

