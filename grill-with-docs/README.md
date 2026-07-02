# grill-with-docs

Research and stress-test a plan against the repo's actual code, domain language, docs, and architectural decisions, then update the docs while decisions crystallize.

This is the docs/spec route, not a mandatory precursor to every goal. Use `grill-with-docs` to establish language, principles, ADRs, and agent guidance; use `grill-me-goal` instead when the main output should be a long-running acceptance-criteria package. `build-with-fable` can consume the output of either route.

## When to use it

Use this skill when:

- A repo is new or under-documented.
- A plan uses vague or overloaded terms.
- You need `AGENTS.md`, `CLAUDE.md`, `CONTEXT.md`, or ADRs before implementation.
- A design needs to be challenged against actual code and existing decisions.
- Future agents need durable context instead of chat-only decisions.

## What it updates

- `CONTEXT.md` / `CONTEXT-MAP.md` for domain language and bounded contexts.
- `docs/adr/` for consequential architecture decisions.
- `AGENTS.md` and `CLAUDE.md` for agent-facing project rules.
- Question logs under `docs/grilling/` for substantial sessions.
- Existing README or docs when foundations, commands, or principles change.

## Core behavior

1. Research the repo before asking avoidable questions.
2. Use bounded subagent side missions when available.
3. Ask 2-10 rounds of batched multiple-choice questions, with stronger defaults for substantial work: 4-6 normal, 6-8 new repo/foundation, 7-10 high-risk or long-running.
4. Never silently assume option A or recommended defaults.
5. Track questions in a question log when the session is substantial.
6. Challenge fuzzy language against code, docs, and concrete scenarios.
7. Establish `AGENTS.md` / `CLAUDE.md` before moving further in new repos.
8. Create ADRs only for decisions that are hard to reverse, surprising without context, and the result of real trade-offs.

## New repo foundation gate

Before implementation, the skill establishes:

- Product purpose and non-goals.
- Language/runtime/framework/package manager.
- Folder structure and module boundaries.
- Test, TDD, E2E, and physical verification expectations.
- Build/run commands.
- Deployment, CI, observability, and rollback expectations.
- Agent working rules.

## Install

```bash
npx skills add e4stwood/agentic-skills
# or
cp -R grill-with-docs ~/.claude/skills/
cp -R grill-with-docs ~/.codex/skills/
```

Invoke with `/grill-with-docs` in Claude Code or `$grill-with-docs` in Codex.
