# Harness for Claude

A minimal repository harness for Claude Code-driven work.

The goal is to give every future task a predictable starting point:

- one place for agent instructions (`CLAUDE.md`)
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

## Claude Code Integration

- `CLAUDE.md` is loaded automatically as project context for every Claude Code session.
- `.claude/settings.json` holds project-shared settings (permissions, env, hooks) that are checked in.
- `.claude/settings.local.json` holds your personal overrides and is git-ignored.
- `.claude/commands/` can hold project-scoped slash commands.
- `.claude/agents/` can hold project-scoped sub-agents.
