# Agent Guidance

This repository contains reusable agent skills. Treat the repo as a distribution artifact: every change should make the skills easier to discover, install, and run correctly in a fresh agent session.

## Purpose

`agentic-skills` packages practical workflows for:

- Research-first planning and documentation.
- Long-running goal execution.
- Fable-led / Codex-assisted implementation.
- Post-change ripple analysis.

## Repo Structure

```text
<skill-name>/
  SKILL.md
  README.md
  agents/openai.yaml

README.md
CLAUDE.md
AGENTS.md
CONTRIBUTING.md
```

## Project Principles

- **Skills are executable guidance.** Write instructions another agent can follow without hidden chat context.
- **Keep `SKILL.md` focused.** Put only instructions needed at runtime. Human-facing explanation belongs in `README.md`.
- **Preserve evidence gates.** Do not weaken TDD, physical verification, subagent boundaries, or "evidence before done" language.
- **Recommendations are not defaults.** Any skill that asks questions must not silently assume option A or recommended choices.
- **Repo docs must stay in sync.** When adding or changing a skill, update root `README.md`, the skill README, and UI metadata.
- **Prefer primary sources for external tools.** If a skill references external CLI/plugin behavior, verify against upstream docs when changing it.

## Commands

Validate a skill:

```bash
python3 ~/.codex/skills/.system/skill-creator/scripts/quick_validate.py <skill-folder>
```

Validate all skills:

```bash
for skill in build-with-fable grill-with-docs grill-me-goal ripple-check; do
  python3 ~/.codex/skills/.system/skill-creator/scripts/quick_validate.py "$skill"
done
```

Find placeholders:

```bash
rg -n "TODO|\\[TODO\\]|placeholder|Structuring This Skill" .
```

Check diff hygiene:

```bash
git diff --check
git status --short --branch
```

## Skill Metadata

Each skill must have:

- `SKILL.md` with YAML frontmatter containing only `name` and `description`.
- `README.md` explaining what the skill does, when to use it, and how to install it.
- `agents/openai.yaml` with `display_name`, `short_description`, and `default_prompt`.

## Documentation Rules

- Root `README.md` is the user-facing catalog and install guide.
- `AGENTS.md` is cross-agent operating guidance.
- `CLAUDE.md` is Claude-specific guidance and may refer back to `AGENTS.md`.
- `CONTRIBUTING.md` is the human workflow for changing, validating, and releasing skills.
- Do not add duplicate docs that create competing sources of truth.
- Keep examples concrete: commands, paths, output files, and decision rules.

## Review Bar

Before reporting a documentation or skill update complete:

- Validate changed skills.
- Confirm README and skill behavior agree.
- Confirm local install copies are synced when requested.
- Commit and push only when the user asks for publishing or the current workflow requires it.
