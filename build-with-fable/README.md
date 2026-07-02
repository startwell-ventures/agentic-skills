# build-with-fable

Set up and run a **Fable-led, Codex-assisted** build workflow.

Fable owns orchestration, taste, product/architecture judgment, decomposition, and final synthesis. Codex owns well-specified implementation, computer use, UI/browser verification, reviews, rescue tasks, and evidence collection.

## When to use it

Use this skill for:

- Substantial implementation work.
- User-facing UI, copy, product flow, or API design.
- Ambiguous architecture or product trade-offs.
- Multi-file work that benefits from parallel research, implementation, testing, and review.
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

## How the workflow runs

1. **Fable plans.** Fable decomposes the work, identifies risks, and decides what needs taste or judgment.
2. **Codex executes.** Codex handles clear-spec implementation, browser/computer-use verification, reviews, rescue tasks, and evidence collection.
3. **Delegated work returns evidence.** Every handoff asks for files, commands, diffs, screenshots, logs, failures, and uncertainties.
4. **Fable judges.** Fable reopens important cited files, resolves conflicts, rejects mediocre output, and decides whether to escalate.
5. **Codex reviews.** Non-trivial changes get Codex review; risky choices get adversarial review.

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
