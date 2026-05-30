# Contributing

Thanks for taking the time to contribute. This repo is small on purpose — keep
PRs scoped and the harness will stay easy to adopt.

## Ground rules

- Read `AGENTS.md` first. It applies to humans and agents alike.
- Don't add features, refactors, or abstractions beyond what an issue asks for.
- Run `scripts/check` before opening a PR. The Stop hook runs it automatically
  during Claude Code sessions; CI runs it too.
- Update or add tests when behavior changes.
- Record durable decisions in `docs/decisions.md`.

## Workflow

1. Open an issue describing the change (skip for trivial typo fixes).
2. Branch from `main`.
3. Make the change and run `scripts/check`.
4. Open a PR using the template in `.github/pull_request_template.md`.

## What's in scope

- Improvements to the agent guide (`AGENTS.md`) that generalize across stacks.
- Safer or clearer Claude Code defaults in `.claude/settings.json`.
- Bug fixes to `scripts/bootstrap`, `scripts/check`, `scripts/test`.
- Documentation of new patterns in `docs/references.md`.

## What's out of scope

- Bundling specific stacks, frameworks, or MCP servers by default.
- Adding sub-agents that duplicate the built-in `Explore` / `Plan` agents.
- Devcontainer / Docker harness (see Trail of Bits' devcontainer instead).

If you're unsure, open an issue and ask before writing the PR.
