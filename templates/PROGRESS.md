# Progress

Live state of this project. **Read on every session start. Update
after every merged PR or material change.**

This file is the cursor; `<PLACEHOLDER: link to plan / roadmap doc if
one exists>` is the plan; `<PLACEHOLDER: link to design doc>` is the
architecture.

## Cursor

**`<PLACEHOLDER: one sentence describing where the project is right
now. Example: "v0.1.0 on main. Phases 1-2 closed. Currently building
slice 3 of v0.2.0 (the auth refactor).">`**

## Current milestone

`<PLACEHOLDER: a few bullets describing the in-flight milestone. What
ships, what gates the close. Example:>`

- Slice 1: `<short description>` — `<status>`
- Slice 2: `<short description>` — `<status>`
- Slice 3: `<short description>` — in flight

Exit criteria for closing this milestone:

- `<criterion 1>`
- `<criterion 2>`

## Recent context

Last ~10 meaningful events. Most recent first. Trim back to 10 when
adding new entries.

- `<YYYY-MM-DD>` — `<one-line description with PR number>`

## Open milestones / backlog

`<PLACEHOLDER: 3-7 bullets, each a milestone or larger slice. Each
bullet is one sentence; details live in the plan doc.>`

- `<milestone name>` — `<one-line description>`

## Open decisions

Questions the project still needs to answer. Move resolved ones to
Recent context or to the design doc.

- `<open question>`

## Update protocol

After a PR merges or a material decision changes:

1. **Tick the milestone** and append the PR number in parens.
2. **Advance the cursor** if the PR landed it. The cursor is one
   sentence: "M*N* complete. Next: M*N+1*."
3. **Add one line to Recent context.** What changed in a single
   sentence, with the PR number. Then trim Recent context back to
   ~10 entries.
4. **Move resolved decisions** out of "Open decisions" — either
   into Recent context or, if they're permanent architectural
   choices, into the design doc and link from here.
5. **Don't duplicate git history.** PR descriptions cover the
   *what*; this file covers the *where we are*.

When in doubt: shorter is better. This file is consulted constantly;
the cost of stale lines is high.