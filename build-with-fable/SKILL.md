---
name: build-with-fable
description: Set up and run a Fable-led, Codex-assisted build workflow in the current repo. Use when the user wants Fable to orchestrate architecture, product judgment, taste, planning, a repo-local build spec, a 50-item execution queue, efficient subagents, and final synthesis while Codex handles well-specified implementation, computer use, UI/browser verification, reviews, rescue tasks, and evidence collection through codex-plugin-cc or Codex CLI.
---

# Build With Fable

## Overview

Use Fable as the orchestrator and final judge. Use Codex as the execution and verification partner. This skill combines the BuilderIO `efficient-fable` pattern with the OpenAI `codex-plugin-cc` workflow and the local model-routing principles from the user's CLAUDE.md.

The outcome is not just a one-off plan. First, install the workflow into the current repo's `CLAUDE.md` if it is missing. Then create or refresh the build spec, plan the next 50 build items when the work is substantial, and run the build through self-contained delegation packets, evidence gates, subagent side missions, Codex execution, and Fable synthesis.

## Forward Progress Mandate

Every turn must materially advance the build. If the user asks for status, answer briefly and then perform, queue, or identify the next concrete action unless they explicitly ask to pause, stop, or only report status.

Forward progress means one or more of:

- Creating or tightening the build spec.
- Planning or replenishing the next 50 build items.
- Launching bounded subagent/Codex side missions.
- Implementing one build item.
- Verifying a build item through tests and real end-to-end evidence.
- Updating `build-queue.md`, `verification.md`, `execution-log.md`, risks, or decisions.
- Removing a blocker or documenting the smallest unblock path.

If blocked, record the blocker, pick the next unblocked queue item, and keep going. Stop only for missing credentials, destructive actions, production risk, or a user decision that cannot be safely converted into an assumption and validation item.

## Setup Gate

Before doing substantive work, inspect the current repo for `CLAUDE.md`.

- If `CLAUDE.md` does not exist, create it.
- If it exists but lacks a "Build With Fable + Codex" section, patch it.
- If it exists with a stale or weaker version, update it.
- Do not duplicate conflicting model-routing instructions. Reconcile and preserve useful local guidance.

The repo guidance must include:

- Fable owns orchestration, product/architecture judgment, taste, and final synthesis.
- Codex owns well-specified implementation, UI/browser verification, computer use, code review, adversarial review, rescue/debug passes, and evidence collection.
- `codex-plugin-cc` setup and command usage.
- Direct Codex CLI fallback with self-contained prompts.
- The model-routing table and escalation principles.
- Handoff packet requirements.
- Evidence and verification rules.
- The build spec + 50-item execution queue gate below.

## Spec + 50-Item Queue Gate

Before implementation, Fable must decide whether an adequate build spec already exists. It should normally consume one upstream spec source: `grill-with-docs` docs/spec output, a `grill-me-goal` package, or another adequate existing spec. It does not require both `grill-with-docs` and `grill-me-goal`.

Look for repo-local sources such as:

- `docs/<goal-slug>/goal.md`, `ac.md`, and `test-plan.md` from `grill-me-goal`.
- `CONTEXT.md`, `AGENTS.md`, `CLAUDE.md`, ADRs, and question logs produced by `grill-with-docs`.
- Existing `PRD.md`, `SPEC.md`, `PLAN.md`, issue specs, roadmap docs, or task lists.
- README/AGENTS/CLAUDE guidance that defines foundations, commands, and acceptance criteria.

A spec is adequate only if it names the concrete outcome, non-goals, acceptance criteria, test/run commands, end-to-end verification path, risks, dependencies, and evidence expectations. If repo foundations are also missing, apply a `grill-with-docs`-style foundation preflight internally before implementation. If the spec is missing or vague, create a build package before coding. This is the first material deliverable: do not let Fable jump into implementation from a weak prompt, and do not require the user to issue separate setup commands.

Default package:

```text
docs/<build-slug>/
  spec.md            # Objective, non-goals, user-visible outcome, success/rejection criteria
  build-plan.md      # Phases, sequencing, architecture/product choices, milestones
  build-queue.md     # The next 50 build items, with status and evidence
  context.md         # Repo-grounded files, commands, flows, tests, constraints
  research.md        # External/API facts, official sources, versions, access dates
  test-plan.md       # TDD, integration, E2E, browser/computer-use, full-suite commands
  delegation.md      # Subagent/Codex lanes, prompts, packets, stop conditions
  verification.md    # Evidence ledger: commands, screenshots, logs, reviews, manual runs
  decisions.md       # Durable decisions and tradeoffs
  risks.md           # Risks, mitigations, owners, linked build items
  execution-log.md   # Wave-by-wave progress, red/green/refactor, E2E proof, next focus
```

If a `grill-me-goal` package already exists, use it as the source of truth and add `build-plan.md`, `build-queue.md`, `delegation.md`, and `verification.md` only if they are missing. Do not fork competing plans.

### The 50 Build Items

For substantial work, the first build artifact must be `build-queue.md` with the next 50 build items. If the entire job is clearly smaller than 50 items, list the full set and state why fewer items cover the whole job. Do not pad with fake work.

The first 10 items must be immediately executable. The remaining 40 can be less detailed, but each must still name a real outcome, dependency, and verification path. After every execution wave, replenish the queue back toward 50 until fewer than 50 items remain in the whole project.

Each item should use this shape:

```markdown
- [ ] BI-001 - <specific end state>
  - Status: ready | blocked | in-progress | review | verified | superseded
  - Outcome: <observable behavior or repo state>
  - Scope: <files, modules, page, API, migration, doc, or test surface>
  - Owner lane: Fable | Codex | subagent:<lane> | human
  - Delegation target: <codex review/rescue/exec, browser verification, wrapper agent, local>
  - Dependencies: <BI ids or none>
  - Test-first: <failing test to write first, or non-behavioral reason>
  - Verification: <command, browser path, manual run, screenshot/log/review evidence>
  - Evidence: <fill before checking>
  - Stop conditions: <when the worker must stop and report back>
```

Queue rules:

- Prefer vertical, end-to-end build slices over horizontal chores.
- Include foundation items first: run commands, test harness, seed data, auth/test accounts, feature flags, CI, and local environment.
- Include polish and verification items, not just implementation.
- Include docs/ops/rollout/rollback/monitoring items when they affect whether the build can ship.
- If a material decision is unknown, ask a batched multiple-choice question round, record the answer in `decisions.md`, and convert unanswered assumptions into validation items. Do not silently assume the recommended option.
- Never check an item without evidence. Never delete unfinished items to look done; mark them superseded with a replacement.
- If the user asks to "just build", create the package and queue, then start the first execution wave unless they explicitly asked for planning only.

## Core Principle

Fable decides what matters. Codex proves it works.

Use Fable for:

- Ambiguous decomposition and strategy.
- Product, architecture, API, UX, and copy judgment.
- Splitting work into independent lanes.
- Resolving conflicts between subagent/Codex reports.
- Reviewing final implementation quality and user-facing taste.
- Deciding whether a cheaper model's output is good enough.

Use Codex for:

- Bounded implementation from clear specs.
- Computer use, browser checks, screenshots, and UI/UX verification.
- Code review and adversarial review.
- Debugging/rescue passes.
- Test runs, log reduction, and evidence collection.
- Bulk/mechanical edits, migrations, data analysis, and repo scans.

## Model Routing Defaults

Treat these as defaults, not limits. Judge the output, not the price tag. If a cheaper model misses the bar, rerun or redo with a smarter model without asking.

| model | cost | intelligence | taste |
| --- | --- | --- | --- |
| gpt-5.5 | 9 | 8 | 5 |
| sonnet-5 | 5 | 5 | 7 |
| opus-4.8 | 4 | 7 | 8 |
| fable-5 | 2 | 9 | 9 |

Rules:

- Cost is a tie-breaker only. For anything that ships: intelligence > taste > cost.
- Anything user-facing (UI, copy, API design, product flows) needs taste >= 7.
- Reviews of plans or implementations should use fable-5 or opus-4.8, optionally gpt-5.5 as an extra independent perspective.
- Bulk/mechanical work (clear-spec implementation, data analysis, migrations, repetitive edits) should go to Codex/gpt-5.5 when available.
- Never use Haiku.
- Claude models such as sonnet-5, opus-4.8, and fable-5 run via Agent/Workflow model parameters.
- gpt-5.5 is reached through Codex: `codex exec`, `codex review`, `codex-plugin-cc`, or a thin Claude wrapper that calls Codex.

## Codex Plugin Setup

When running inside Claude Code, prefer `openai/codex-plugin-cc`.

Expected setup commands:

```bash
/plugin marketplace add openai/codex-plugin-cc
/plugin install codex@openai-codex
/reload-plugins
/codex:setup
```

If Codex is missing, install it:

```bash
npm install -g @openai/codex
```

If Codex is installed but not authenticated:

```bash
!codex login
```

Useful plugin commands:

- `/codex:review --background` for read-only Codex review.
- `/codex:adversarial-review --background <focus>` for steerable challenge review.
- `/codex:rescue --background <task>` for delegated bug investigation or bounded implementation.
- `/codex:status` and `/codex:result` to manage background jobs.
- `/codex:transfer` when work should continue directly in Codex.

Use `/codex:setup --enable-review-gate` only when the user is actively monitoring the loop; review gates can drain usage quickly.

## Direct Codex CLI Fallback

When plugin commands are unavailable, use the Codex CLI directly:

```bash
codex review
codex exec -s read-only "<self-contained prompt>"
```

For direct execution, prompts must be self-contained. Include repo path, objective, files/surfaces in scope, out-of-scope boundaries, verification commands, evidence required, and stop conditions.

## Thin Wrapper Pattern

When a Claude workflow/subagent cannot directly select a Codex model, spawn a low-effort Claude wrapper whose only job is to call Codex.

Wrapper prompt shape:

```text
You are a thin wrapper. Do not solve the task yourself.
Write a self-contained Codex prompt for the task below, run it with `codex exec` or the available /codex command, then return Codex's result, evidence, files changed, and unresolved issues.

Task:
<bounded task>

Repo:
<absolute repo path>

Stop if Codex needs broader scope, missing credentials, destructive actions, or unclear requirements.
```

## Workflow

1. Run the Setup Gate and update `CLAUDE.md`.
2. Run the Spec + 50-Item Queue Gate. Do not jump into code if the spec is weak or missing.
3. Inspect repo docs, current branch, changed files, tests, commands, constraints, and external/API specs. Use internet research for any current external fact, preferably official sources.
4. Launch efficient side missions when available: codebase map, test/run harness, UX/product flow, external API specs, risk/ops, and verification strategy. Keep Fable responsible for synthesis.
5. Create or refresh `build-plan.md`, `build-queue.md`, `test-plan.md`, `delegation.md`, and `verification.md`.
6. Execute in waves:
   - Select 3-8 unblocked `BI-*` items from the current phase, or one item if it is risky.
   - Assign bounded lanes to Codex/subagents with handoff packets tied to item IDs.
   - Integrate results by reopening cited files, reviewing diffs, and resolving conflicts.
   - Run the stated tests and a real end-to-end path for feature-bearing work.
   - Record evidence in `verification.md`, update `build-queue.md`, and append `execution-log.md`.
   - Replenish the queue back toward 50 with newly discovered work.
7. Use Codex for UI/browser/computer-use verification whenever the task has user-facing behavior.
8. Run Codex review for non-trivial changes and adversarial review for risky architecture, data, auth, caching, concurrency, migration, or rollback choices.
9. If output is mediocre, rerun with a better model or narrower prompt. Do not ship mediocre work because it was cheaper.
10. Keep moving: if one item blocks, document the blocker and take the next unblocked build item. Stop only for missing credentials, destructive actions, user decisions, or risk that cannot be safely bounded.
11. Finish with evidence: commands, screenshots, logs, diffs, review results, queue status, shipped items, and unresolved risks.

## Handoff Packets

Every delegated task must include:

- Repo path.
- Build package path and `BI-*` item IDs.
- Exact objective.
- In-scope files, components, pages, APIs, or commands.
- Explicit out-of-scope boundaries.
- Current assumptions.
- Acceptance criteria.
- Verification commands or browser paths.
- Evidence format to return.
- Stop conditions.

Good stop conditions:

- The code does not match the prompt's assumptions.
- The task needs out-of-scope files.
- A command fails after one reasonable retry.
- Credentials, secrets, production access, or destructive actions are needed.
- The agent finds a safer plan than the prompt allows.

## Review Bar

Do not outsource responsibility. Treat delegated reports as leads.

Before declaring work done:

- The build spec exists or an adequate existing spec is cited.
- `build-queue.md` exists for substantial work, with the first 10 items executable and queue status updated.
- `execution-log.md` and `verification.md` record what happened in the latest wave.
- Fable or the orchestrator reopens important cited files.
- Codex review has run for non-trivial code changes.
- Codex adversarial review has run for risky architecture, data, auth, caching, or concurrency decisions.
- User-facing flows have been physically verified with browser/computer-use evidence.
- The final summary includes what was delegated, what was verified, what changed, and what remains risky.

## When To Skip

Skip this workflow for tiny, local, low-risk fixes where delegation overhead is bigger than the task.

Do not skip it for:

- UI/UX work.
- Multi-file implementation.
- Ambiguous product or architecture decisions.
- PRs that will ship to users.
- Work that previously would have produced throwaway end-to-end PRs.
