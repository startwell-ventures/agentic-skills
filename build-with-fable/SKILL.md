---
name: build-with-fable
description: Set up and run a Fable-led, Codex-assisted build workflow in the current repo. Use when the user wants Fable to orchestrate architecture, product judgment, taste, planning, and final synthesis while Codex handles well-specified implementation, computer use, UI/browser verification, reviews, rescue tasks, and evidence collection through codex-plugin-cc or Codex CLI.
---

# Build With Fable

## Overview

Use Fable as the orchestrator and final judge. Use Codex as the execution and verification partner. This skill combines the BuilderIO `efficient-fable` pattern with the OpenAI `codex-plugin-cc` workflow and the local model-routing principles from the user's CLAUDE.md.

The outcome is not just a one-off plan. First, install the workflow into the current repo's `CLAUDE.md` if it is missing. Then run the build using self-contained delegation packets, evidence gates, and explicit model routing.

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
2. Inspect repo docs, current branch, changed files, tests, commands, and constraints.
3. Let Fable decompose the work into lanes: plan, implementation, verification, review, and polish.
4. Delegate bounded lanes to Codex or cheaper agents using handoff packets.
5. Keep Fable responsible for synthesis, conflict resolution, taste, and final review.
6. Use Codex for UI/browser/computer-use verification whenever the task has user-facing behavior.
7. Run Codex review and, for risky choices, adversarial review before shipping.
8. Reopen important cited files and verify delegated claims before treating them as true.
9. If output is mediocre, rerun with a better model or narrower prompt. Do not ship mediocre work because it was cheaper.
10. Finish with evidence: commands, screenshots, logs, diffs, review results, and unresolved risks.

## Handoff Packets

Every delegated task must include:

- Repo path.
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
