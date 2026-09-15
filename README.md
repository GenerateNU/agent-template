# agent-template

Shared engineering principles for GenerateNU projects, in the format coding
agents actually read.

Every project writes down the same things — layer boundaries, error handling,
pagination rules, test expectations — and every project writes them slightly
differently, in a `CONTRIBUTING.md` that agents never open. This repo keeps one
copy, versioned, so a fix lands everywhere instead of in one repo.

## What's here

| Path | What it is |
| --- | --- |
| `core/core.md` | Stack-agnostic principles. Always loaded. ~120 lines. |
| `template/project-header.md` | The per-project sections you fill in: commands, architecture, stack, local conventions. |

## Install

From the root of your project:

```bash
TPL=/path/to/agent-template
cat $TPL/template/project-header.md $TPL/core/core.md > AGENTS.md
echo '@AGENTS.md' > CLAUDE.md
```

Then **fill in the TODO sections at the top of `AGENTS.md`** — commands,
architecture, stack, project conventions. The shared principles below the
divider are already done.

### Why two files

`AGENTS.md` is the cross-tool convention: Cursor and Codex read it directly.
Claude Code does *not* read `AGENTS.md` — it reads `CLAUDE.md` — so the one-line
`CLAUDE.md` imports it. One source of truth, both tools work.

If you need Claude-specific instructions, add them below the import:

```markdown
@AGENTS.md

## Contributing

Open a PR. One rule per PR, and the description must name **the agent failure it
prevents** — an actual thing that went wrong in an actual repo. If nobody can
name the failure, the rule doesn't go in. That's the only thing keeping this
file from growing to 600 lines of generic advice nobody reads.

Fixes go here, not in your project's copy. Editing the shared section in one
repo means the next project inherits the bug.

## Updating

The install is a copy, so a project doesn't pick up changes automatically. Each
generated `AGENTS.md` carries its version in an HTML comment
(`<!-- agent-template · core v0.1 -->`). To see what a project is missing:

```bash
git diff core-v0.1..main -- core/core.md
```

Apply the parts you want. Tag a new version here whenever `core.md` changes in
a way projects should know about.

## Sources

Mined from the `CLAUDE.md`, `docs/`, and `CONTRIBUTING.md` of
[toggo](https://github.com/GenerateNU/toggo),
[dearly](https://github.com/GenerateNU/dearly), and
[selfserve](https://github.com/GenerateNU/selfserve). Every rule in `core.md`
is something at least two of the three already agreed on.
