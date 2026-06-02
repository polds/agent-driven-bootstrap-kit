# The bootstrap prompt

Copy everything between the `=== BEGIN ===` and `=== END ===` markers
below and paste it as the first message in a fresh agent session
running inside the target repository.

---

```
=== BEGIN AGENT-DRIVEN DEVELOPMENT BOOTSTRAP ===

You are bootstrapping a long-lived, agent-driven development workflow
for this repository. This is the first session; nothing is set up yet.

# What you are going to do

1. Interview me (6-8 questions, one batch) to learn the project.
2. Based on my answers, create a scaffold of always-read context
   files so future agent sessions can pick up cold without
   re-deriving project state every time.
3. Stop after the scaffold lands. Don't start building features
   until I ask.

# The scaffold you will produce

Create these files in the repository root unless I tell you otherwise:

- **CLAUDE.md** — the always-read entry point. Lists what to read
  first, the tech stack, working conventions, and any sub-agents and
  skills available. Future sessions read this on every start.
- **PROGRESS.md** — the cursor. One section says "where we are right
  now" in a single sentence; one section lists recent context (~10
  lines, trimmed); one section lists open milestones. Has an "Update
  protocol" section at the bottom describing how to keep it current.
- **TENETS.md** — non-negotiable principles, numbered. Cite numbers
  in PRs and commit messages. Start with 5-8 seeded tenets that fit
  the project; expect to add more as you push back on me during
  development.
- **.claude/agents/** (or the equivalent location for the agent
  runner being used) — sub-agent definitions for recurring
  specialized work. Start with 4-5 generic ones and add more later.
  - `developer.md` — primary implementation agent for the project's
    main language/stack.
  - `code-reviewer.md` — independent diff review against tenets.
  - `researcher.md` — library/prior-art surveys.
  - `devops-engineer.md` — CI, build, deploy, release automation.
  - `sre.md` — observability, alerting, runbooks (skip if the
    project doesn't run as a service).

# Conventions the scaffold encodes

Write these into CLAUDE.md so future sessions inherit them.

- **One PR per slice.** A slice is the smallest change that ships
  value end-to-end. Split anything that touches more than ~400 lines
  or more than ~3 concerns into multiple PRs.
- **Draft PRs by default.** Open as draft; mark ready when CI is
  green and the description is filled in.
- **Branch naming.** `<agent-prefix>/<slice-name>` (e.g.
  `agent/auth-rate-limit`). Lower-case kebab.
- **Commit messages.** Multi-paragraph body: what changed, why now,
  trade-offs considered. Cite tenet numbers and design-doc sections
  when the change is load-bearing. Subject line under 72 chars.
- **PR descriptions.** Three sections: `Summary`, `Test plan` (as a
  checklist), and (when relevant) status of the larger slice
  series. Cross-reference tenets and design-doc sections.
- **Per-directory CLAUDE.md** for any package that grows past ~3
  files OR accumulates non-obvious conventions. Each lists: layout,
  invariants, conventions, tests, and a maintenance hook. When you
  add, rename, or remove a file in such a directory, update the
  local CLAUDE.md in the same PR. Drift here is worse than absence.
- **Update PROGRESS.md after every merged PR.** The file's "Update
  protocol" section spells out what to change.
- **Webhook loop, not polling.** If the agent runner supports PR
  activity subscriptions, the operator subscribes the session to a
  PR; the agent responds to CI failures and review comments as
  events arrive rather than polling for status.

# The interview

Ask me these questions in a single message, one numbered list, so I
can answer them in one batch. Adapt the wording to fit context you
can see from the repo (e.g. existing `package.json` tells you it's
JavaScript; don't ask about language). If the repo is empty, ask
everything.

1. **Project name and one-paragraph description.** What is this
   thing, who is it for?
2. **Primary language and stack.** Language, runtime, framework,
   database. Tell me what's locked in vs. open.
3. **Deployment target.** Local CLI? Long-running service? Library
   on a package registry? Mobile app? Determines whether `sre.md`
   and operational tenets apply.
4. **Quality bar.** What does "done" mean? Production grade? Internal
   tool? Throwaway research code? Tests required, or is a manual
   smoke-test enough?
5. **Current state.** Is this a fresh repo or an existing codebase?
   If existing, what's the rough size and what does the entry point
   look like?
6. **Top three project-specific tenets.** Things you know you'll
   push back on if I violate them — "no magic strings", "no
   network calls in unit tests", "PRs must include a runbook
   update", etc. I will seed the rest with generics; these are the
   ones that are non-negotiable for *this* project.
7. **Specialized agents needed beyond the defaults.** Domains where
   you'd want a dedicated sub-agent — e.g. a `data-engineer.md`
   for a heavy-SQL project, a `prompt-engineer.md` for an LLM
   product, a `kubernetes-expert.md` for an infra-heavy project.
   "None for now" is a fine answer; we can add them later.
8. **Agent runner.** Which agent are you using to drive this?
   (Claude Code, Cursor, an Agent SDK app, etc.) Affects only the
   path conventions for sub-agent definitions and the webhook setup.

Wait for my answers. Then build the scaffold in one pass — multiple
file writes are fine in a single turn. Use the templates at
.bootstrap/templates/ in this repo as starting shapes; fill in
the parts that depend on my answers; cut sections that don't apply.

When the scaffold is in place, summarize what landed and stop. Do
not start the first feature PR until I tell you to.

=== END AGENT-DRIVEN DEVELOPMENT BOOTSTRAP ===
```

---

## After the bootstrap lands

Once the agent has built the scaffold, the day-to-day flow is in
[`workflow.md`](workflow.md). The short version:

- Ask for a slice. The agent plans, opens a draft PR, subscribes the
  session to the PR's activity, and lands the change.
- Review the diff. Comment on the PR; the agent picks up review
  comments as webhook events and pushes follow-up commits.
- Merge. The agent updates `PROGRESS.md`, you ask for the next
  slice.

The scaffold's job is to make every session after the first one cheap
to start. The agent reads `PROGRESS.md` + `CLAUDE.md` + the
per-directory `CLAUDE.md` for whichever package it's about to touch,
and is up to speed in seconds rather than re-greppping for ten minutes.