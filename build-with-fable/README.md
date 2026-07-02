# build-with-fable

Set up and run a **Fable-led, Codex-assisted** build workflow.

Fable owns orchestration, taste, product/architecture judgment, decomposition, and final synthesis. Codex owns well-specified implementation, computer use, UI/browser verification, reviews, rescue tasks, and evidence collection.

## What it does

- Adds or updates the repo's `CLAUDE.md` with a "Build With Fable + Codex" process.
- Applies the BuilderIO `efficient-fable` pattern: Fable decides, cheaper agents do bounded heavy lifting, evidence flows back.
- Uses OpenAI's `codex-plugin-cc` commands for review, adversarial review, rescue, status/result, and transfer.
- Falls back to direct `codex exec` / `codex review` with self-contained prompts.
- Encodes the user's model-routing policy: judge output, not price; user-facing work needs taste; Codex is preferred for computer use and verification.

## Install

```bash
npx skills add e4stwood/agentic-skills
# or
cp -R build-with-fable ~/.claude/skills/
cp -R build-with-fable ~/.codex/skills/
```

Invoke with `/build-with-fable` in Claude Code or `$build-with-fable` in Codex.
