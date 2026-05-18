# Harness for Claude

A minimal repository harness for Claude Code-driven work.

The goal is to give every future task a predictable starting point:

- one place for agent instructions (`AGENTS.md`, mirrored as `CLAUDE.md`)
- one bootstrap command
- one check command
- one test command
- lightweight documentation for decisions and tasks
- a shared `.claude/` directory for project-level settings, slash commands, and sub-agents

## Quick Start

```sh
scripts/bootstrap
scripts/check
```

## Workflow

1. Write the task brief in `tasks/` when the work needs context.
2. Implement changes in the relevant project files.
3. Record durable decisions in `docs/decisions.md`.
4. Run `scripts/check` before finishing.

## Automation Entrypoints

- `scripts/bootstrap`: install or prepare dependencies when a known stack is present.
- `scripts/check`: run formatting, linting, type checks, and tests when available.
- `scripts/test`: run the test suite when available.

These scripts are safe defaults for an empty or early-stage repository. Extend
them as the project grows.

## Agent Configuration

`AGENTS.md` is the canonical agent guide (the cross-tool spec used by Codex,
Cursor, Aider, and others). `CLAUDE.md` is a symlink to `AGENTS.md` so Claude
Code reads the same content. **Edit `AGENTS.md` only.**

> Windows note: git stores the symlink with mode `120000`. On Windows checkouts
> the symlink works only with `git config --global core.symlinks true` and
> Developer Mode (or admin) enabled — otherwise `CLAUDE.md` appears as a plain
> text file containing the string `AGENTS.md`. If that's a problem for your
> team, replace the symlink with a copy and add a pre-commit hook to sync them.

## Claude Code Integration

- `AGENTS.md` / `CLAUDE.md` are loaded automatically as project context for every Claude Code session.
- `CLAUDE.local.md` (git-ignored) holds personal overrides.
- `.claude/settings.json` holds project-shared settings (permissions, hooks) that are checked in.
- `.claude/settings.local.json` holds your personal overrides and is git-ignored. See `.claude/settings.local.json.example` for a starter (statusLine, etc.).
- `.claude/commands/` holds project-scoped slash commands.
- `.claude/hooks/` holds lifecycle scripts. The defaults are:
  - `session-start-context` — injects branch + uncommitted state + open task notes when interesting.
  - `stop-check` — runs `scripts/check` before session end and blocks on failure.
- `.claude/agents/` can hold project-scoped sub-agents (none ship by default).
- `.github/pull_request_template.md` aligns PR descriptions with the verification checklist Claude follows.

See `docs/references.md` for upstream Claude Code documentation.
