# CLAUDE.md

Follow `AGENTS.md` as the cross-agent source of truth. This file contains Claude-specific guidance, especially for Fable-led / Codex-assisted build work.

## Repo Rules

- Keep skills concise and executable.
- Validate changed skills with `quick_validate.py`.
- Update root `README.md` and the relevant skill README when public behavior changes.
- Preserve the model-routing, evidence, and verification bars below.

## Build With Fable + Codex

Use this workflow for substantial build work, user-facing changes, codebase-heavy tasks, or anything that benefits from parallel research, implementation, verification, and review.

### Model routing

Rankings are local workflow defaults, not hard limits. Cost reflects what I actually pay, not list price. Intelligence is how hard a problem can be handled unsupervised. Taste covers UI/UX, code quality, API design, and copy.

| model | cost | intelligence | taste |
| --- | --- | --- | --- |
| gpt-5.5 | 9 | 8 | 5 |
| sonnet-5 | 5 | 5 | 7 |
| opus-4.8 | 4 | 7 | 8 |
| fable-5 | 2 | 9 | 9 |

How to apply:

- These are defaults, not limits. If a cheaper model's output does not meet the bar, rerun or redo the work with a smarter model without asking.
- Judge the output, not the price tag. Escalating costs less than shipping mediocre work.
- Cost is a tie-breaker only. For anything that ships: intelligence > taste > cost.
- Bulk/mechanical work such as clear-spec implementation, data analysis, migrations, and repetitive edits should go to Codex/gpt-5.5 when available.
- Anything user-facing such as UI, copy, API design, and product flows needs taste >= 7.
- Reviews of plans or implementations should use fable-5 or opus-4.8, optionally gpt-5.5 as an extra independent perspective.
- Never use Haiku.

### Roles

- Fable is the orchestrator, architect, product/taste judge, conflict resolver, and final synthesizer.
- Codex is the execution and verification partner for well-specified implementation, computer use, UI/browser verification, code review, adversarial review, rescue/debug passes, log reduction, and evidence collection.
- Do not outsource responsibility. Treat delegated reports as leads until important claims are verified against cited files, commands, screenshots, or logs.

### Spec and execution queue

Before implementation, Fable checks whether an adequate repo-local spec already exists. A usable spec names the outcome, non-goals, acceptance criteria, test/run commands, end-to-end verification path, risks, dependencies, and evidence expectations.

If the spec is missing or weak, Fable's first material output is a build package under `docs/<build-slug>/`:

```text
spec.md
build-plan.md
build-queue.md
context.md
research.md
test-plan.md
delegation.md
verification.md
decisions.md
risks.md
execution-log.md
```

For substantial work, `build-queue.md` starts with the next 50 build items. The first 10 must be immediately executable; the remaining items still need outcomes, dependencies, and verification paths. Each execution wave selects 3-8 unblocked items, delegates bounded work to Codex/subagents, integrates results, verifies end to end, records evidence, and replenishes the queue toward 50 until fewer than 50 real items remain.

Every turn should make forward progress: tighten the spec, plan/replenish queue items, launch a bounded side mission, implement an item, verify evidence, update docs, or document the smallest unblock path and move to the next unblocked item.

### Codex access from Claude Code

Prefer OpenAI's Codex plugin for Claude Code.

Setup:

```bash
/plugin marketplace add openai/codex-plugin-cc
/plugin install codex@openai-codex
/reload-plugins
/codex:setup
```

If Codex is missing:

```bash
npm install -g @openai/codex
```

If Codex is not authenticated:

```bash
!codex login
```

Useful commands:

- `/codex:review --background`
- `/codex:adversarial-review --background <focus>`
- `/codex:rescue --background <bounded task>`
- `/codex:status`
- `/codex:result`
- `/codex:transfer`

Use `/codex:setup --enable-review-gate` only when actively monitoring the session.

### Direct Codex CLI fallback

If plugin commands are unavailable, use Codex directly:

```bash
codex review
codex exec -s read-only "<self-contained prompt>"
```

For work not covered by review/rescue commands, run `codex exec -s read-only` with a self-contained prompt.

### Thin wrapper pattern

When a Claude workflow/subagent cannot directly select a Codex model, spawn a low-effort Claude wrapper whose only job is to call Codex and return the result.

Wrapper prompt:

```text
You are a thin wrapper. Do not solve the task yourself.
Write a self-contained Codex prompt for the task below, run it with `codex exec` or the available /codex command, then return Codex's result, evidence, files changed, and unresolved issues.

Task:
<bounded task>

Repo:
<absolute repo path>

Stop if Codex needs broader scope, missing credentials, destructive actions, or unclear requirements.
```

### Delegation packet requirements

Every delegated task must include:

- Repo path.
- Exact objective.
- In-scope files, components, pages, APIs, or commands.
- Explicit out-of-scope boundaries.
- Assumptions.
- Acceptance criteria.
- Verification commands or browser paths.
- Evidence format to return.
- Stop conditions.

### Review and verification bar

- Use Codex for computer-use and UI/browser verification when the work is user-facing.
- Run Codex review for non-trivial code changes.
- Run Codex adversarial review for risky architecture, data, auth, caching, concurrency, or rollback decisions.
- Reopen important cited files before trusting delegated claims.
- If verification is weak, narrow the prompt and rerun with a better model or stronger evidence requirement.
- Do not ship mediocre work because it was cheaper.
