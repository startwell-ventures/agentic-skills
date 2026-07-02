# agentic-skills

Reusable agent skills for agentic execution, repo foundations, long-running coding tasks, and model orchestration. The collection is tuned for Claude Code and Codex, with a strong bias toward research-first planning, test-first implementation, physical verification, and evidence before done.

## Philosophy

These skills are built around a few operating rules:

- **Research first.** Inspect the repo, docs, tests, CI, and external primary sources before inventing plans.
- **Ask better questions.** Use dense, multi-round multiple-choice grilling when decisions matter. Recommendations are advisory, not silent defaults.
- **Document durable truth.** Put language, principles, decisions, goals, evidence, and handoff context in repo files, not only in chat.
- **Use the right model for the job.** Let Fable own judgment and taste. Let Codex own well-scoped implementation, computer use, UI/browser verification, reviews, and evidence.
- **TDD and verify end to end.** Behavior changes need failing-test-first work and real app/browser/CLI verification where relevant.
- **Evidence before done.** Check boxes, phase gates, and final reports only after concrete proof exists.

## Skills

| Skill | Use it when | Main output |
| --- | --- | --- |
| [build-with-fable](./build-with-fable) | You want Fable to orchestrate substantial implementation while Codex handles well-specified execution, computer use, verification, reviews, and rescue tasks. | A repo-level Fable + Codex workflow, build spec package, 50-item execution queue, delegation plan, and evidence ledger. |
| [grill-with-docs](./grill-with-docs) | You need to stress-test a plan against repo reality, sharpen language, establish foundations, or update agent-facing docs before implementation. | Updated `CONTEXT.md`, `AGENTS.md`, `CLAUDE.md`, ADRs, question logs, and related docs. |
| [grill-me-goal](./grill-me-goal) | You want to turn a vague or long-running objective into a durable goal package with detailed acceptance criteria. | `docs/<goal-slug>/` with `goal.md`, `ac.md`, `context.md`, `research.md`, `test-plan.md`, and execution logs. |
| [ripple-check](./ripple-check) | You changed code and need to find/fix the blast radius before shipping. | Test-first ripple fixes, validation results, and docs updates. |

## Recommended Flow

For a new feature, repo, or multi-session project, choose **one upstream specification route** first:

- **Docs/spec route: `grill-with-docs`** - use when the work needs repo foundations, language, ADRs, agent guidance, or a conventional spec before implementation.
- **Goal route: `grill-me-goal`** - use instead when the work should become a long-running goal package with durable acceptance criteria and resume prompts.

Then:

1. **`build-with-fable`** consumes the output of either route: `grill-with-docs` docs/specs, a `grill-me-goal` package, or another adequate existing spec. If no adequate spec exists, it creates a build package and 50-item execution queue before coding.
2. **`ripple-check`** runs after implementation changes to audit the blast radius test-first.

For a brand-new or under-documented repo, the chosen upstream route should perform a `grill-with-docs`-style foundation preflight internally before implementation. The user should not need to know to issue a separate command.

For a small local fix, skip the orchestration and use the narrowest relevant skill.

## Install

Install the whole collection with the Agent Skills CLI:

```bash
npx skills add e4stwood/agentic-skills
```

Or copy a single skill folder into an agent's skills directory:

```bash
cp -R build-with-fable ~/.claude/skills/
cp -R grill-with-docs ~/.claude/skills/
cp -R grill-me-goal ~/.codex/skills/
cp -R ripple-check ~/.codex/skills/
```

Depending on the agent runtime, local installs may live under `~/.codex/skills/`, `~/.claude/skills/`, or `~/.agents/skills/`.

Invoke with `/skill-name` in Claude Code or `$skill-name` in Codex.

## Recommended Companions

- **[mattpocock/skills](https://github.com/mattpocock/skills)** - source of the `/tdd` skill and the original `grill-with-docs` inspiration.
- **Codex long-running `/goal`** - `grill-me-goal` produces the package; Codex `/goal` executes it over long, multi-session runs.
- **[openai/codex-plugin-cc](https://github.com/openai/codex-plugin-cc)** - lets Claude Code call Codex for reviews, adversarial reviews, rescue tasks, status/result polling, and transfer into Codex.

## Skill Selection

Use this quick route:

- **"We need foundations, language, principles, ADRs, or CLAUDE/AGENTS docs."** Use `grill-with-docs`.
- **"We need a hundred-hour checklist with acceptance criteria."** Use `grill-me-goal`.
- **"We already have a spec/goal package and need Fable to run the build while Codex executes and verifies."** Use `build-with-fable`.
- **"We changed code and need to know what else broke."** Use `ripple-check`.

## Repo Structure

```text
<skill-name>/
  SKILL.md              # Required skill metadata and instructions
  README.md             # Human-facing guide for the skill
  agents/openai.yaml    # Codex/OpenAI UI metadata

CLAUDE.md               # Claude-specific workflow guidance
AGENTS.md               # Cross-agent repo guidance
CONTRIBUTING.md         # Human workflow for changing and releasing skills
README.md               # Collection overview
```

## Development

See [CONTRIBUTING.md](./CONTRIBUTING.md) for the full change workflow.

When adding or changing a skill:

1. Update `SKILL.md`.
2. Update the skill's `README.md`.
3. Update root `README.md` if the public behavior or catalog changes.
4. Update `AGENTS.md` / `CLAUDE.md` if agent behavior changes.
5. Validate the skill:

```bash
python3 ~/.codex/skills/.system/skill-creator/scripts/quick_validate.py <skill-folder>
```

6. Scan for placeholders:

```bash
rg -n "TODO|\\[TODO\\]|placeholder|Structuring This Skill" .
```

7. Check markdown/diff hygiene:

```bash
git diff --check
git status --short --branch
```

## Local Sync

This repo is the source of truth. To refresh local installs manually:

```bash
cp -R build-with-fable ~/.codex/skills/
cp -R build-with-fable ~/.claude/skills/
cp -R build-with-fable ~/.agents/skills/
cp -R grill-with-docs ~/.codex/skills/
cp -R grill-with-docs ~/.claude/skills/
cp -R grill-with-docs ~/.agents/skills/
cp -R grill-me-goal ~/.codex/skills/
cp -R grill-me-goal ~/.claude/skills/
cp -R grill-me-goal ~/.agents/skills/
cp -R ripple-check ~/.codex/skills/
cp -R ripple-check ~/.claude/skills/
cp -R ripple-check ~/.agents/skills/
```

Prefer installing via `npx skills add e4stwood/agentic-skills` for normal use.

## Release Checklist

Before pushing:

- `quick_validate.py` passes for every changed skill.
- Root README and the skill README agree.
- `agents/openai.yaml` has a clear `display_name`, `short_description`, and `default_prompt`.
- `CLAUDE.md` / `AGENTS.md` match the actual workflow.
- No placeholder text remains.
- The git diff only contains intended files.
