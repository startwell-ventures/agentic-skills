# grill-with-docs

Research and stress-test a plan against the repo's actual code, domain language, docs, and architectural decisions, then update the docs while decisions crystallize.

This is the foundation-setting companion to `grill-me-goal`: use `grill-with-docs` to establish language, principles, ADRs, and agent guidance; use `grill-me-goal` when the work is ready to become a long-running acceptance-criteria package.

## What it updates

- `CONTEXT.md` / `CONTEXT-MAP.md` for domain language and bounded contexts
- `docs/adr/` for consequential architecture decisions
- `AGENTS.md` and `CLAUDE.md` for agent-facing project rules
- Existing README or docs when foundations, commands, or principles change

## Core behavior

1. Research the repo before asking avoidable questions.
2. Use bounded subagent side missions when available.
3. Ask 2-10 rounds of batched multiple-choice questions, with stronger defaults for substantial work: 4-6 normal, 6-8 new repo/foundation, 7-10 high-risk or long-running.
4. Never silently assume option A or recommended defaults.
5. Track questions in a question log when the session is substantial.
6. Challenge fuzzy language against code, docs, and concrete scenarios.
7. Establish `AGENTS.md` / `CLAUDE.md` before moving further in new repos.
8. Create ADRs only for decisions that are hard to reverse, surprising without context, and the result of real trade-offs.

## Install

```bash
npx skills add e4stwood/agentic-skills
# or
cp -R grill-with-docs ~/.claude/skills/
cp -R grill-with-docs ~/.codex/skills/
```

Invoke with `/grill-with-docs` in Claude Code or `$grill-with-docs` in Codex.
