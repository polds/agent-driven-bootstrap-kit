# `<package-path>` — `<one-line role>`

<!--
Template for a per-directory CLAUDE.md. Add one when a package grows
past ~3 files OR accumulates non-obvious conventions. Replace the
PLACEHOLDER sections; delete this comment block once filled in.
-->

`<PLACEHOLDER: 1-3 sentence description of what this package does and
where it sits in the architecture.>`

**Source of truth:** `<PLACEHOLDER: cross-reference to design doc
sections, audit IDs, or related documents.>`

## Layout

| File | Role |
|---|---|
| `<filename>` | `<short description>` |
| `<filename>` | `<short description>` |

## Invariants

Things that must remain true. Future agents touching this package
should be able to add a new feature *here* by reading this list
alone, without re-deriving the constraints from code.

- **`<invariant 1>`** — `<short statement + why it matters>`
- **`<invariant 2>`** — `<short statement>`
- **`<invariant 3>`** — `<short statement>`

## Conventions

How code in this package is normally shaped. Defaults; deviate when
the case justifies it.

- `<convention 1>`
- `<convention 2>`

## Tests

`<PLACEHOLDER: notes on how tests in this package are structured.
Where the fakes live, what's gated on an external dependency, etc.>`

## Maintenance

When you add, rename, or remove a file in this directory — or change
a load-bearing invariant — update this CLAUDE.md so the layout and
invariants stay accurate. `<PLACEHOLDER: callout for the single most
important invariant a new agent should not break.>`