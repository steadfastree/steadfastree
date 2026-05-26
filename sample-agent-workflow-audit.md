# Sample: AI Agent Workflow Audit

This is a redacted sample of the written output I can provide for a repository that is trying to use AI coding agents in real development work.

## Scope

- Repository type: TypeScript product/tooling repo
- Agent tools: Claude Code, Codex, Cursor, Copilot, or a custom runner
- Goal: reduce failed runs, duplicate work, vague PRs, and maintainer review load
- Output: prioritized findings, concrete templates, and a short implementation path

## Executive summary

The repository is close to being agent-friendly, but the workflow currently depends on chat memory and maintainer corrections. The highest-leverage fix is not more automation. It is a tighter contract between the issue, the agent instructions, the PR checklist, and the review gate.

Recommended first pass:

1. Add one root-level `AGENTS.md` that names the canonical workflow.
2. Add an agent-ready issue template with acceptance criteria, non-goals, likely files, and required verification.
3. Add a PR template that forces agents to report changed files, tests run, screenshots/logs, and known risks.
4. Split “implementation” and “review” into separate passes so the same agent is not silently approving itself.

## Finding 1 — Instructions are scattered

### Symptoms

- `README.md`, issue comments, prompt snippets, and local notes disagree.
- Agents follow old setup instructions or miss repo-specific constraints.
- Maintainers repeat the same rules in PR comments.

### Why it matters

AI coding agents are literal about the context they see. If the repository has three partially correct instruction sources, the agent will often choose the most recent-looking one rather than the most authoritative one.

### Fix

Create a short root-level `AGENTS.md` and make it the entry point:

```md
# Agent instructions

## Start here

1. Read the linked issue fully.
2. Restate acceptance criteria before editing.
3. Do not start implementation if scope, commands, or data access are unclear.

## Commands

- Install: `pnpm install`
- Typecheck: `pnpm typecheck`
- Test: `pnpm test`

## Before opening a PR

- Summarize changed files.
- List verification commands and results.
- State known risks and rollback notes.
- Do not mark complete if required verification failed.
```

Keep long product context under `docs/agent/` and link it from `AGENTS.md` instead of turning the root file into a dump.

## Finding 2 — Issues are not agent-ready

### Symptoms

- The agent starts coding before constraints are clear.
- PRs pass tests but miss product intent.
- The review thread becomes the real specification.

### Fix

Use an issue template like this:

```yaml
name: Agent-ready task
description: A scoped task for a coding agent
body:
  - type: textarea
    id: context
    attributes:
      label: Context
      description: What problem should the agent solve?
    validations:
      required: true
  - type: textarea
    id: non_goals
    attributes:
      label: Non-goals
      description: What should the agent avoid changing?
    validations:
      required: true
  - type: textarea
    id: acceptance
    attributes:
      label: Acceptance criteria
    validations:
      required: true
  - type: textarea
    id: verification
    attributes:
      label: Required verification
      description: Exact commands, screenshots, or logs expected before PR.
    validations:
      required: true
```

## Finding 3 — Review is not independent enough

### Symptoms

- The same agent implements and “reviews” its own diff.
- PR descriptions say “tests pass” without naming commands.
- Risky changes to auth, billing, migrations, or data flows get reviewed like normal UI copy changes.

### Fix

Add a PR review packet:

```md
## Summary

## Changed files

## Verification

- [ ] `pnpm typecheck`
- [ ] `pnpm test`
- [ ] Manual check / screenshot / log attached if applicable

## Risk checklist

- [ ] Auth/session behavior touched
- [ ] Data migration or schema touched
- [ ] Payment/billing touched
- [ ] Background job or cron touched
- [ ] Public API response touched

## Known risks / rollback
```

If the repo uses multiple agents, reserve one clean pass for review only. The reviewer should compare the diff against the issue, not against the implementer’s explanation.

## What a real paid audit would include

For a small public or sanitized repository, I can usually provide:

- 5–10 prioritized workflow findings;
- suggested `AGENTS.md` / `CLAUDE.md` structure;
- issue and PR template drafts;
- a lightweight review-gate checklist;
- recommended next actions split into “same day”, “this week”, and “later”.

A real audit does not require private credentials or production data. If private details are needed, they should be shared only after scope is agreed and through an appropriate private channel.
