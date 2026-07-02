# ripple-check

You just changed the codebase. `ripple-check` maps and contains the **blast radius**: a deep ripple-effect analysis across every internal consumer and external dependency, fixing each ripple **test-first**, extending coverage over externalities, running full validation, and updating docs.

Run it after editing code and before committing or shipping.

## When to use it

Use this skill when:

- You changed public types, API shapes, data models, config, auth, caching, or workflows.
- The change may affect frontend, backend, tests, migrations, queues, jobs, webhooks, or observability.
- You need to know whether a fix has hidden downstream breakage.
- You want a test-first containment pass before review.

Skip it only for tiny changes with no behavioral or contract impact.

## What it checks

In parallel where possible, beyond in-process code, across externalities:

- Databases and migrations: forward/backward safety, backfills, nullability.
- Queues, event streams, and webhooks: payload versioning and idempotency.
- Third-party APIs/SDKs and your own published API contract.
- Config, secrets, and feature flags, including the flag-off path.
- Caching and invalidation.
- Observability: logs, metrics, traces, dashboards, alerts.
- Security/compliance, performance, concurrency, retries, rate limits.
- Build, CI, and infrastructure-as-code.

## TDD over every ripple

Every fix follows the `/tdd` loop:

1. **RED** - write a failing test that reproduces the ripple.
2. **GREEN** - make the minimal change to pass.
3. **REFACTOR** - clean up with the suite green.

No fix-first-test-later. Never refactor while red. Tests should verify behavior through public interfaces. Each loop should leave a test that would have caught the regression.

## Final evidence

The final report should include:

- Ripple effects found.
- Externalities checked.
- Fixes applied.
- Tests added or updated.
- Validation commands and results.
- Docs updated.
- Human action needed.

## Install

```bash
npx skills add e4stwood/agentic-skills
# or
cp -R ripple-check ~/.codex/skills/
cp -R ripple-check ~/.claude/skills/
```

Invoke with `/ripple-check` in Claude Code or `$ripple-check` in Codex.
