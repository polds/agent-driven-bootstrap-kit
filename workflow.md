# The operating model

What the bootstrapped scaffold encodes, in long form. Read this when
you want to understand *why* a file exists or *why* the prompt phrases
something the way it does.

## The shape of a session

Every agent session starts cold. The agent has no memory of prior
sessions; whatever state matters has to live on disk and be reachable
from a small number of always-read files.

The scaffold's job is to make "what's the current state and what's
next?" answerable in under 60 seconds of file reads.

### Session-start reading list

In order:

1. **PROGRESS.md** — one section says where you are right now in a
   single sentence ("the cursor"). One section lists the last ~10
   meaningful events. One section lists open milestones. If this is
   honest and current, the rest of the session moves fast.
2. **CLAUDE.md** — the conventions. Read on every session because
   sub-agents may have evolved, tenet count may have grown, build
   commands may have changed.
3. **TENETS.md** — the principles. Cite numbers in commit messages
   and PR descriptions.
4. **The relevant per-directory CLAUDE.md** for whichever package
   the session is about to touch. Local invariants, layout, and
   "where to look" notes are here.

That's it. Everything else (design docs, audit notes, runbooks) is
on-demand reading triggered by what the cursor says or what the
slice requires.

## The PR-per-slice flow

A *slice* is the smallest change that ships value end-to-end. The
unit isn't a feature or a story — it's the smallest atomic landing
that leaves the codebase in a coherent state.

### Sizing rules of thumb

- A slice that touches more than ~3 concerns is two slices.
- A slice that's more than ~400 lines of real diff is two slices.
- A slice that needs a schema migration AND new business logic AND
  new tests is at least two slices: ship the schema first, then
  the logic that uses it.

### PR lifecycle

1. **Plan.** Agent reads the relevant context (PROGRESS, CLAUDE,
   relevant per-dir CLAUDE), drafts a short plan, asks the operator
   if it's not obvious which slice comes next.
2. **Branch + work.** New branch with the agent's namespace prefix
   (e.g. `agent/<slice-name>`). Small commits are fine; the squash
   on merge handles cleanup.
3. **Open as draft.** Always draft. The PR description has three
   sections:
   - **Summary** — what changed and why now, in 1-3 paragraphs.
     Cite tenets and design-doc sections where the change is
     load-bearing.
   - **Test plan** — checklist of what should pass: tests, CI jobs,
     manual smoke-checks.
   - **Slice status** — if this is part of a series ("PR 2 of 3"),
     note where in the series this lands and what's next.
4. **Subscribe to PR activity.** If the agent runner supports it,
   subscribe the session to the PR. The webhook loop replaces
   polling.
5. **Respond to events.** CI failures get diagnosed and fixed.
   Review comments get answered with a commit or a reply. The
   agent pushes; the operator reviews; loop until merge.
6. **After merge.** Update PROGRESS.md. Tick the milestone. Trim
   "Recent context" back to ~10 lines.

### Commit message conventions

- Subject line: under 72 chars, imperative ("add", "fix", "rename"),
  no trailing period.
- Body: multi-paragraph. What changed. Why now. Trade-offs
  considered or invariants preserved. Cite tenet numbers and
  design-doc sections when the change is non-obvious.
- One commit per logical step. Don't fold three concerns into one
  commit just because you can.

## The maintenance rule

This is the single most important convention. Write it into the
top-level CLAUDE.md verbatim:

> When you add, rename, or remove a file in a directory that has a
> CLAUDE.md, update that file in the same PR. Drift in these files
> is worse than absence — they're load-bearing for context handoff.
> Add a new directory-level CLAUDE.md when a package grows past ~3
> files or starts accumulating non-obvious conventions.

Without this, the per-directory files rot and start actively
misleading future sessions. With this, each PR carries the
maintenance overhead of the change it's making, which is small.

## When to add per-directory CLAUDE.md

A package earns one when:

- It has more than ~3 files AND there's something non-obvious about
  the layout, idioms, or invariants. (3 files of pure boilerplate
  don't need a CLAUDE.md.)
- It has a load-bearing invariant that's easy to violate (e.g. "all
  mutations must go through this gate"). The CLAUDE.md states the
  invariant once so it doesn't have to be inferred from code.
- A reviewer or sub-agent has been re-greppping the same layout
  questions across multiple sessions. The grep cost is the signal.

Don't pre-emptively add per-dir CLAUDE.md to every package. They
accumulate as the project's complexity does.

## Sub-agents

Used for specialized recurring work. Each agent is a short Markdown
file describing what it knows and how it works. The runner invokes
the agent with a one-shot prompt; the agent returns a result.

Good candidates for a sub-agent:

- **Language specialist** — idiomatic implementation in the
  project's main language (`go-developer`, `rust-developer`,
  `python-developer`, etc.).
- **Code reviewer** — independent review of a diff against the
  tenets and design doc. Useful for second opinions.
- **Researcher** — library evaluation, prior-art surveys, focused
  documentation reading.
- **DevOps** — CI, build, deploy, release automation, secrets.
- **SRE** — observability, alerting, runbooks (only for services).
- **Domain experts** — whatever recurring specialized work the
  project actually does (LLM orchestration, K8s, data engineering,
  security, etc.).

Add a sub-agent only when you'd genuinely use it more than ~3
times. A sub-agent file no one calls is overhead.

## The webhook loop

If the agent runner can subscribe to PR activity (CI events, review
comments), use it. Polling for status burns context and is brittle.

Subscribed flow:

1. Agent opens PR, subscribes the session.
2. Agent ends turn (no polling).
3. CI failure webhook → agent investigates, pushes a fix.
4. Review comment webhook → agent answers via commit or reply.
5. Merge webhook → unsubscribe, update PROGRESS.md.

If the runner doesn't support subscriptions, the operator must
explicitly ask "check the PR" when they want a status update. The
agent should never sleep-and-poll.

## What to push back on

The scaffold encodes a set of opinions. Some are non-negotiable;
others are defaults. Push back on:

- **"Just one more thing in this PR."** Slices stay small. If
  scope creep is real, open the next PR.
- **"Skip the test, we're moving fast."** Tests are the scaffold's
  immune system. If the test is genuinely not worth writing, say
  why explicitly.
- **"Don't bother updating PROGRESS.md, it's tedious."** That file
  is the cursor. Without it, every session restarts cold.
- **"This sub-agent is overkill for this task."** Maybe. If the
  task is one-off, just do it directly. If it's recurring, the
  sub-agent pays for itself by session 3.

## What to deviate on

Not every project should look like this. Reasonable deviations:

- **No `sre.md`** for libraries and CLIs that don't run as services.
- **No `devops-engineer.md`** for tiny projects without CI.
- **Different tenets** for projects with different values. (A
  research repo's tenets look nothing like a payments service's.)
- **Different branch prefix** if your team uses a different
  convention.
- **Skip per-dir CLAUDE.md entirely** if the project is small
  enough that grep covers it.

The goal is durable context handoff between sessions. Whatever
shape achieves that for your project is the right shape.