# Contributing

This repo is a distribution artifact for reusable agent skills. A good change should make a skill easier to discover, install, understand, and run correctly from a fresh agent session.

## Change Types

- **Skill behavior change:** update `SKILL.md`, the skill `README.md`, root `README.md` if public behavior changed, and `agents/openai.yaml` if the default prompt or metadata changed.
- **Documentation-only change:** update the smallest set of docs that removes ambiguity, then check that entry points link to the updated material.
- **New skill:** create `<skill-name>/SKILL.md`, `<skill-name>/README.md`, and `<skill-name>/agents/openai.yaml`, then add the skill to root `README.md`.
- **Workflow guidance change:** update `AGENTS.md` for cross-agent rules and `CLAUDE.md` only for Claude-specific guidance.

## Skill Authoring Rules

- Keep `SKILL.md` executable and concise. Runtime instructions belong there; background explanation belongs in `README.md`.
- Preserve strong evidence gates: research first, batched questions where needed, test-first implementation, end-to-end verification, and explicit proof before reporting done.
- Do not silently choose recommended answers for users. If a skill asks questions, recommendations are advisory until the user answers.
- Use concrete paths, commands, file outputs, and stop conditions. Avoid vague instructions like "do the right thing."
- Prefer official docs and primary sources when documenting external tools, SDKs, APIs, or plugin behavior.

## Required Files

Each skill folder should contain:

```text
<skill-name>/
  SKILL.md
  README.md
  agents/openai.yaml
```

`SKILL.md` frontmatter should contain only:

```yaml
---
name: skill-name
description: One clear trigger-focused description.
---
```

## Validation

Validate every changed skill:

```bash
python3 ~/.codex/skills/.system/skill-creator/scripts/quick_validate.py <skill-folder>
```

Validate all current skills:

```bash
for skill in build-with-fable grill-with-docs grill-me-goal ripple-check; do
  python3 ~/.codex/skills/.system/skill-creator/scripts/quick_validate.py "$skill"
done
```

Scan for unfinished template text:

```bash
rg -n "TODO|\\[TODO\\]|placeholder|Structuring This Skill" .
```

Check the diff before committing:

```bash
git diff --check
git status --short --branch
```

## Local Install Smoke Test

For normal use, prefer:

```bash
npx skills add startwell-ventures/agentic-skills
```

When testing local changes, copy only the changed skill into the agent runtime you are validating:

```bash
cp -R <skill-name> ~/.codex/skills/
cp -R <skill-name> ~/.claude/skills/
cp -R <skill-name> ~/.agents/skills/
```

Not every machine uses all three directories. Use the runtime directories configured for the current agent environment.

## Release Bar

Before pushing:

- Changed skills validate with `quick_validate.py`.
- Root `README.md`, the skill README, and `SKILL.md` agree on behavior.
- `AGENTS.md` and `CLAUDE.md` do not contain competing guidance.
- Install instructions are accurate for Codex and Claude-style runtimes.
- Placeholder scans are clean or only hit intentional examples.
- The diff contains only intended docs, metadata, or skill files.
