# Bootstrap: agent-driven development workflow

A copy-pasteable starter kit for running long-lived, agent-driven
software projects out of any repository. The workflow it sets up is
the one this repo runs day-to-day — small PRs, durable context files,
explicit invariants, sub-agents for specialized work, and a webhook
loop that keeps CI green without operator polling.

## Quick start

**Prerequisites:** a git repository and an agent that can write files
and run shell commands. [Claude Code](https://claude.ai/code) is the
reference implementation; Cursor and similar tools work too.

**Step 1 — Copy the templates into your target repository.**

The agent needs the scaffold templates at build time. From this kit,
copy the `templates/` directory into your target repo as `.bootstrap/`:

```
cp -r /path/to/agent-driven-bootstrap-kit/templates your-project/.bootstrap/templates
```

**Step 2 — Open the agent in your target repository.**

```
cd your-project
claude   # or however you start your agent
```

**Step 3 — Paste the bootstrap prompt.**

Open [`prompt.md`](prompt.md) and copy everything between the
`=== BEGIN ===` and `=== END ===` markers. Paste it as the **first
message** in a fresh session.

**Step 4 — Answer the interview.**

The agent sends back 6–8 questions in one batch. Answer them in a
single reply. The questions cover: project name and description,
language and stack, deployment target, quality bar, current repo
state, your top three non-negotiable tenets, any specialized
sub-agents you want, and which agent runner you're using.

**Step 5 — Let the agent build the scaffold.**

The agent reads the templates from `.bootstrap/templates/` and writes
the filled-in scaffold files in a single turn:

| File | What it is |
|---|---|
| `CLAUDE.md` | Always-read entry point. Stack, conventions, sub-agents. |
| `PROGRESS.md` | The cursor. Where the project is right now + open milestones. |
| `TENETS.md` | Non-negotiable principles, numbered. Cited in every PR. |
| `.claude/agents/` | Sub-agent definitions for recurring specialized work. |

**Step 6 — Review, clean up, and commit.**

Read through the generated files. Edit anything that doesn't match
your intent — the agent's first draft is a starting point, not a
contract. Then remove the temporary template directory and commit.

```
rm -rf .bootstrap/
git add CLAUDE.md PROGRESS.md TENETS.md .claude/
git commit -m "bootstrap: add agent-driven workflow scaffold"
```

That's it. Every future session reads `PROGRESS.md` + `CLAUDE.md`
and is up to speed in under 60 seconds.

---

## Day-to-day after the bootstrap

Once the scaffold is in place, the flow from [`workflow.md`](workflow.md) is:

1. **Ask for a slice.** The agent reads context, plans the smallest
   coherent change, and opens a draft PR.
2. **Review the diff.** Comment on the PR; if the agent runner
   supports PR webhooks, it picks up review comments and CI failures
   as events and responds without being prompted.
3. **Merge.** The agent updates `PROGRESS.md`. Ask for the next
   slice.

The scaffold keeps growing as the project does: tenets get added when
you push back on the agent, per-directory `CLAUDE.md` files appear
when a package accumulates non-obvious conventions, and sub-agents
get added when a category of work recurs.

---

## When to use this

Use this when you want an autonomous or semi-autonomous coding agent
to drive sustained development across many sessions. The scaffold
solves the "every session starts from zero" problem by giving the
agent a small set of always-read files that carry working state.

Skip this for one-off scripts, throwaway prototypes, or short-lived
spikes. The overhead pays off only when context loss between sessions
is the dominant cost.

## Files in this bootstrap

| File | Purpose |
|---|---|
| [`prompt.md`](prompt.md) | The copy-pasteable bootstrap prompt. |
| [`workflow.md`](workflow.md) | The day-to-day operating model. Read this to understand *why* the prompt asks what it asks. |
| [`templates/CLAUDE.md`](templates/CLAUDE.md) | Top-level CLAUDE.md template the bootstrap fills in. |
| [`templates/PROGRESS.md`](templates/PROGRESS.md) | Cursor + milestone tracker template. |
| [`templates/TENETS.md`](templates/TENETS.md) | Non-negotiable principles template, with starter tenets. |
| [`templates/per-dir-CLAUDE.md`](templates/per-dir-CLAUDE.md) | Per-directory CLAUDE.md template for packages that grow past ~3 files. |
| `templates/agents/` | Generic sub-agent definitions: [`developer.md`](templates/agents/developer.md), [`code-reviewer.md`](templates/agents/code-reviewer.md), [`researcher.md`](templates/agents/researcher.md), [`devops-engineer.md`](templates/agents/devops-engineer.md), [`sre.md`](templates/agents/sre.md). |

## What this is not

- **Not a project template.** No build files, no CI, no language
  scaffolding — those are project-specific and the agent generates
  them after the interview if you ask.
- **Not a configuration for a specific tool.** The conventions assume
  an agent that can write files, run shell commands, and open PRs.
  Claude Code is the reference; adapt as needed.
- **Not a methodology.** It's a workflow, not a doctrine. Override
  anything that doesn't fit your project.

## License

Public domain / CC0 equivalent — copy, adapt, redistribute freely.
The whole point is that you can lift this into any repo.
