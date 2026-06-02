# CLAUDE.md — Working in this repo

<!--
Template for the top-level CLAUDE.md. Replace placeholder sections
(marked PLACEHOLDER) with project-specific content during bootstrap.
Delete this comment block once filled in.
-->

`<PLACEHOLDER: one-sentence description of what this project is and
who uses it.>`

`<PLACEHOLDER: one sentence on current status — alpha / v0 on main /
production / etc.>`

## Read first, every session

1. [`PROGRESS.md`](PROGRESS.md) — cursor, milestone status, recent context. **Read first.**
2. [`TENETS.md`](TENETS.md) — non-negotiable principles. Cite numbers when justifying choices.

On demand:

- `README.md` — overview and operator quickstart.
- `<PLACEHOLDER: design doc(s) — e.g. DESIGN.md / ARCHITECTURE.md / docs/architecture/*.md>`
- `<PLACEHOLDER: any audit / postmortem docs you reference often>`

## Per-directory CLAUDE.md files

Some directories ship a focused CLAUDE.md with that package's
invariants, layout, and "where to look" notes. Read the relevant one
before touching code in that directory — it's faster than re-grepping
the layout each session.

`<PLACEHOLDER: table of directories with a CLAUDE.md. Start empty;
fill in as packages mature.>`

| Directory | Covers |
|---|---|
| _none yet_ | |

**Maintenance rule (binding on all agents):** when you add, rename, or
remove a file in a directory that has a CLAUDE.md, update that file in
the same PR. Drift in these files is worse than absence — they're
load-bearing for context handoff. Add a new directory-level CLAUDE.md
when a package grows past ~3 files or starts accumulating non-obvious
conventions.

Update `PROGRESS.md` after every merged PR or material decision. The
file's "Update protocol" section spells out what to change. Keeping it
current is part of "done."

## Tech stack

| Area | Choice | Notes |
|---|---|---|
| Language | `<PLACEHOLDER>` | |
| Runtime | `<PLACEHOLDER>` | |
| Storage | `<PLACEHOLDER>` | |
| Deploy target | `<PLACEHOLDER>` | |

## Working conventions

- **Tenets are non-negotiable.** When in doubt, cite the tenet number
  in PR descriptions and commit messages. If a change appears to
  violate a tenet, surface it explicitly rather than smuggling it
  through.
- **Slice-sized PRs.** One PR per slice. A slice is the smallest
  change that ships value end-to-end. Split anything that touches
  more than ~3 concerns or ~400 lines into multiple PRs.
- **Draft by default.** Open PRs as draft. Mark ready when CI is
  green and the description is filled in.
- **Per-directory CLAUDE.md** for packages with non-obvious
  conventions (see maintenance rule above).
- **Update PROGRESS.md** after every merged PR. It's the cursor.

## Style

- `<PLACEHOLDER: line length, formatter, lint rules>`
- Comments: only where the _why_ is non-obvious. Code should explain
  itself; comments explain hidden constraints.

## Build, test, lint

`<PLACEHOLDER: project-specific commands. Examples:>`

| Target | What it does |
|---|---|
| `make build` | Build the binary. |
| `make test` | All tests. |
| `make lint` | Linter (configured in `.golangci.yml` / `.eslintrc` / etc.) |

## Branch and commit conventions

- **Branch name:** `<agent-prefix>/<slice-name>` (kebab-case).
- **Commit subject:** under 72 chars, imperative (`add`, `fix`,
  `rename`), no trailing period.
- **Commit body:** multi-paragraph. What changed, why now,
  trade-offs considered. Cite tenet numbers when the change is
  load-bearing.
- **PR description:** `Summary` + `Test plan` (checklist) +
  optional `Slice status` section.

## Agents available

`<PLACEHOLDER: table of sub-agent definitions in .claude/agents/ (or
the agent runner's equivalent path). Start with the defaults below
and trim or extend per the interview.>`

| Agent | When to use |
|---|---|
| `developer` | Primary implementation, idiomatic code in the project's main language. |
| `code-reviewer` | Independent review of changes against the tenets. |
| `researcher` | Library evaluation, prior-art surveys, technique investigations. |
| `devops-engineer` | CI workflows, container images, release automation. |
| `sre` | SLOs/SLIs, observability, alerting, runbooks, capacity planning. |

## Skills available

`<PLACEHOLDER: optional. Skills are short reusable command-style
helpers a session can invoke. Examples from a previous project:
tenet-check, schema-sync. Start empty if you don't need any.>`

| Skill | Purpose |
|---|---|
| _none yet_ | |