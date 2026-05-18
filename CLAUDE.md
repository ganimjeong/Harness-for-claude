# Claude Harness Instructions

This repository is a baseline workspace for Claude Code-driven implementation.
It is intentionally language-agnostic — extend it as a concrete stack is added.

## Operating Principles

- Read the repository before editing. Prefer existing conventions over new ones.
- Keep changes scoped to the requested task. Do not add features, refactors, or
  abstractions that were not asked for.
- Do not revert user changes unless explicitly asked.
- Use `rg` for searching when available.
- Use `scripts/bootstrap` before first development when dependencies are present.
- Use `scripts/check` before handing work back.
- Add or update tests when behavior changes.
- Document decisions that affect future work in `docs/decisions.md`.

## Repository Layout

- `CLAUDE.md`: This file. Project-level instructions auto-loaded by Claude Code.
- `README.md`: Human-facing overview and workflow.
- `docs/`: Project notes and decisions.
- `scripts/`: Stable automation entrypoints (`bootstrap`, `check`, `test`).
- `tasks/`: Task briefs and working notes — start from `tasks/TEMPLATE.md`.
- `.claude/`: Claude Code project configuration.
  - `settings.json`: Shared, checked-in settings (permissions, env, hooks).
  - `settings.local.json`: Personal overrides (git-ignored).
  - `commands/`: Project slash commands.
  - `agents/`: Project sub-agents.

## Standard Commands

```sh
scripts/bootstrap   # install or prepare dependencies
scripts/check       # lint, typecheck, and test when available
scripts/test        # tests only
```

These scripts auto-detect Node, Python, and Rust stacks. When a new stack is
introduced, extend the scripts rather than bypassing them.

## Completion Criteria

Before finishing a task:

1. Confirm the requested change is implemented.
2. Run the narrowest useful verification command, usually `scripts/check`.
3. Report changed files and any verification that could not be run.
