# Sample: AI Agent Workflow Audit

This is a sample of the kind of written output I can provide for a repository that is trying to use AI coding agents in production work.

## Scope

- Repository type: TypeScript product/tooling repo
- Agent tools: Claude Code, Codex, Cursor, or custom agent runners
- Goal: reduce failed agent runs and improve handoff quality

## Typical findings

### 1. Instructions are scattered

Symptoms:

- `README.md`, project docs, issue comments, and chat messages disagree.
- Agents follow stale setup steps.
- Reviewers need to restate the same rules every PR.

Fix:

- Create one top-level `AGENTS.md` as the entry point.
- Move long domain docs under `docs/agent/` and link them from `AGENTS.md`.
- Keep “must always do” rules short and close to the repo root.

### 2. Issues are too vague for autonomous work

Symptoms:

- Agent starts coding before understanding constraints.
- Tests pass locally but feature misses product intent.
- PR description does not map back to acceptance criteria.

Fix:

Use an issue template with:

- Context
- Non-goals
- Acceptance criteria
- Required tests
- Files likely involved
- Rollback notes

### 3. No independent audit step

Symptoms:

- Same agent writes and reviews its own work.
- Regression risk hides in “looks fine” PRs.

Fix:

- Split implementation and review agents.
- Require a pre-PR packet:
  - diff summary
  - tests run
  - known risks
  - screenshots/logs if applicable

## Example deliverables

### `AGENTS.md` starter

```md
# Agent instructions

## First steps

1. Read this file.
2. Read the linked task issue fully.
3. Confirm the acceptance criteria before editing.

## Commands

- Install: `pnpm install`
- Typecheck: `pnpm typecheck`
- Test: `pnpm test`

## Quality gate

Before PR:

- Run required tests.
- Summarize changed files.
- State known risks.
- Do not mark complete if verification failed.
```

### Issue template starter

```yaml
name: Agent task
description: A task intended for an AI coding agent
body:
  - type: textarea
    id: context
    attributes:
      label: Context
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
    validations:
      required: true
```

## Pricing example

- Small written audit: from KRW 30,000
- Issue/PR template setup: from KRW 50,000
- Async review + written recommendations: from KRW 100,000

To request a real audit, open a paid inquiry issue in this repo.
