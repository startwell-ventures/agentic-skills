# grill-me-goal

Turn a vague long-running objective into a durable, repo-local **goal package**: a complete goal prompt plus docs and a phased, evidence-gated acceptance-criteria checklist that an agent can execute for hundreds of hours across many sessions without relying on chat history.

Use it to **define, sharpen, persist, resume, audit, or run** a multi-session goal.

## Pairs with Codex `/goal`

`grill-me-goal` writes the package; Codex's long-running `/goal` skill executes it by pointing at the generated goal prompt and working the checklist over long, multi-session runs. Use `grill-me-goal` to plan, `/goal` to run.

## When to use it

Use this skill when:

- The work will span many sessions.
- A goal needs dozens or hundreds of acceptance criteria.
- A future agent must resume from repo docs without chat history.
- Scope, risks, tests, and verification need to be nailed down before execution.
- You want progress tracked with evidence rather than vibes.

On a well-founded repo, this can replace `grill-with-docs` as the upstream spec route. On a new or under-documented repo, the skill should perform a `grill-with-docs`-style foundation preflight internally, then capture those foundations inside the goal package before implementation. The user should not need to issue a separate command.

## What it produces

A package under `docs/<goal-slug>/`:

| File | Purpose |
| --- | --- |
| `goal.md` | Source of truth plus complete goal/resume prompt. |
| `ac.md` | Phased checklist, each phase ending in an end-to-end gate. |
| `context.md` | Repo-grounded context from actual inspection. |
| `research.md` | External/API truth with cited primary sources and access dates. |
| `test-plan.md` | TDD and end-to-end strategy with runnable commands per layer. |
| `questions.md` | Batched multiple-choice clarifications plus answers/assumptions. |
| `decisions.md` | Durable decisions affecting execution. |
| `risks.md` | Risk register with mitigations and linked A/Cs. |
| `execution-log.md` | Resume history: red/green/refactor plus end-to-end run per entry. |

## Three non-negotiable doctrines

1. **Test-driven.** Every behavior change is failing-test-first: red, green, refactor. Tests verify behavior through public interfaces.
2. **Evidence before progression.** No A/C or phase gate is checked without concrete proof.
3. **End-to-end completion.** A feature is done only when it is wired and observed through the full user/system path.

## How it works

1. **Research first.** Inspect the repo, docs, tests, CI, run harness, and external/API specs.
2. **Foundation gate.** If the repo lacks foundations, perform a `grill-with-docs`-style foundation preflight internally and capture the missing foundations inside the goal package.
3. **Test/E2E strategy.** Write `test-plan.md` before writing A/Cs.
4. **Batched questions.** Ask 2-10 rounds of multi-choice questions. Recommendations are advisory only.
5. **Write the package.** Large goals should usually have 100-300 phased, test-backed, evidence-gated criteria.
6. **Force forward progress.** The generated goal prompt requires each execution turn to complete a concrete action.
7. **Audit.** A fresh agent must be able to resume from docs alone.

## Companion: `/tdd`

The red/green/refactor core comes from [mattpocock/skills](https://github.com/mattpocock/skills). Install it so the test-first ethos is live during execution:

```bash
npx skills add mattpocock/skills
```

## Install

```bash
npx skills add startwell-ventures/agentic-skills
# or
cp -R grill-me-goal ~/.claude/skills/
cp -R grill-me-goal ~/.codex/skills/
```

Invoke with `/grill-me-goal` in Claude Code or `$grill-me-goal` in Codex.
