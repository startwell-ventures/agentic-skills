# build-with-fable

Set up and run a **Fable-led, Codex-assisted** build workflow with a real execution system: repo-local specs, a 50-item build queue, efficient subagents, Codex implementation/review, and end-to-end evidence.

Fable owns orchestration, taste, product/architecture judgment, decomposition, and final synthesis. Codex owns well-specified implementation, computer use, UI/browser verification, reviews, rescue tasks, and evidence collection.

## When to use it

Use this skill for:

- Substantial implementation work.
- User-facing UI, copy, product flow, or API design.
- Ambiguous architecture or product trade-offs.
- Multi-file work that benefits from parallel research, implementation, testing, and review.
- Work that lacks a strong spec and needs Fable to create the next 50 build items before coding.
- PRs where you would otherwise risk throwing away an end-to-end agent-generated branch.

Skip it for tiny local fixes where delegation overhead is larger than the task.

## What it sets up

The first gate is repo setup. The skill creates or updates `CLAUDE.md` with:

- Fable/Codex role split.
- Model-routing defaults.
- `openai/codex-plugin-cc` setup and commands.
- Direct `codex exec` / `codex review` fallback.
- Thin wrapper pattern for Claude agents that need Codex.
- Delegation packet requirements.
- Review and verification bar.
- A mandatory spec + 50-item execution queue gate.

## Build package

If an adequate spec already exists, Fable uses it. If not, the first material output is a package such as:

```text
docs/<build-slug>/
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

`build-queue.md` contains the next 50 build items for substantial work. The first 10 must be immediately executable; the remaining 40 still need outcomes, dependencies, and verification paths. After each wave, Fable replenishes the queue toward 50 until fewer than 50 real items remain.

## How the workflow runs

1. **Fable keeps forward progress.** Every turn advances the spec, queue, delegation, implementation, verification, docs, or unblock path.
2. **Fable establishes the spec.** If specs are missing or weak, Fable creates the build package before coding.
3. **Fable plans the queue.** Substantial work starts with the next 50 build items, including tests, verification, dependencies, and delegation target.
4. **Fable launches side missions.** Subagents map code, test harnesses, UX flows, external APIs, risks, and verification paths.
5. **Codex executes bounded items.** Codex handles clear-spec implementation, browser/computer-use verification, reviews, rescue tasks, and evidence collection.
6. **Fable integrates waves.** Fable selects 3-8 unblocked build items, integrates results, verifies evidence, updates docs, and replenishes the queue.
7. **Codex reviews.** Non-trivial changes get Codex review; risky choices get adversarial review.
8. **Done means observed end to end.** User-facing work must be physically verified in the running app or real workflow, not just tested in isolation.

## Codex plugin setup

```bash
/plugin marketplace add openai/codex-plugin-cc
/plugin install codex@openai-codex
/reload-plugins
/codex:setup
```

Common commands:

```bash
/codex:review --background
/codex:adversarial-review --background <focus>
/codex:rescue --background <bounded task>
/codex:status
/codex:result
/codex:transfer
```

Fallback:

```bash
codex review
codex exec -s read-only "<self-contained prompt>"
```

## Model routing

Defaults from the workflow:

| model | cost | intelligence | taste |
| --- | --- | --- | --- |
| gpt-5.5 | 9 | 8 | 5 |
| sonnet-5 | 5 | 5 | 7 |
| opus-4.8 | 4 | 7 | 8 |
| fable-5 | 2 | 9 | 9 |

Rules:

- Judge output, not price.
- Cost is only a tie-breaker.
- Anything user-facing needs taste >= 7.
- Use Codex/gpt-5.5 for clear-spec execution, migrations, bulk edits, data analysis, browser checks, and review.
- Use fable-5 or opus-4.8 for plan and implementation reviews.
- Never use Haiku.

## Install

```bash
npx skills add e4stwood/agentic-skills
# or
cp -R build-with-fable ~/.claude/skills/
cp -R build-with-fable ~/.codex/skills/
```

Invoke with `/build-with-fable` in Claude Code or `$build-with-fable` in Codex.
