# Harness for Claude Code — a minimal AGENTS.md + CLAUDE.md starter

[![CI](https://github.com/ganimjeong/Harness-for-claude/actions/workflows/check.yml/badge.svg)](https://github.com/ganimjeong/Harness-for-claude/actions/workflows/check.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)
[![AGENTS.md](https://img.shields.io/badge/spec-AGENTS.md-blue.svg)](https://agents.md)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-ready-8A2BE2.svg)](https://code.claude.com)

A tiny, language-agnostic **harness for Claude Code** (and Codex, Cursor, Aider — any agent that
reads the [AGENTS.md spec](https://agents.md)). Drop it into a new repo and every coding agent
gets the same predictable starting point: one agent guide, one bootstrap, one check, one test.

> Use this if you want a reliable baseline for **AI-agent-driven development** without committing
> to a stack, framework, or vendor lock-in. Extend it as your project grows.

## Why this exists

Most "Claude Code starter" repos are full-stack templates that force a language choice. This one
isn't. It ships only the conventions every project needs:

- A single canonical agent guide (`AGENTS.md`, mirrored as `CLAUDE.md` via symlink)
- Stable script entrypoints (`bootstrap`, `check`, `test`) that auto-detect Node, Python, Rust
- Safe Claude Code defaults: narrow `Bash` allowlist, command-substitution deny rules, Stop-hook
  self-verification, SessionStart context loader
- Lightweight places for decisions and task briefs

If you've ever opened a fresh repo and asked an agent "where do I even start?" — this answers that.

## Quick start

```sh
git clone https://github.com/ganimjeong/Harness-for-claude
cd Harness-for-claude
scripts/bootstrap   # installs deps if a known stack is detected
scripts/check       # lint + typecheck + test, whatever applies
```

Then point Claude Code at the directory — it auto-loads `CLAUDE.md` (→ `AGENTS.md`).

## Features

- **Cross-tool agent guide** — one `AGENTS.md` works with Claude Code, Codex, Cursor, Aider, and
  any tool that follows the [AGENTS.md cross-tool spec](https://agents.md).
- **Self-verifying Stop hook** — `.claude/hooks/stop-check` runs `scripts/check` before the
  session ends and blocks on failure, so regressions don't slip past.
- **SessionStart context loader** — injects branch, uncommitted state, and open task notes only
  when interesting; output capped so it never bloats the context window.
- **Hardened permissions** — narrow `Bash(...)` allowlist plus explicit denies for `.env*`,
  `secrets/`, force-push, and command-substitution patterns (mitigates
  [CVE-2026-35021](https://www.sentinelone.com/vulnerability-database/cve-2026-35021/)).
- **Stack-agnostic scripts** — `scripts/bootstrap`, `scripts/check`, and `scripts/test` detect
  Node, Python, and Rust automatically.
- **Decision log + task briefs** — `docs/decisions.md` and `tasks/TEMPLATE.md` give agents a
  durable place to record what matters and what's next.

## Workflow

1. Drop a task brief into `tasks/` (copy `tasks/TEMPLATE.md`) when context is non-trivial.
2. Implement changes in the relevant project files.
3. Record durable decisions in `docs/decisions.md`.
4. Run `scripts/check` — the Stop hook will run it anyway.

## Agent configuration

`AGENTS.md` is the canonical agent guide. `CLAUDE.md` is a symlink to it so Claude Code reads the
same content. **Edit `AGENTS.md` only.**

> **Windows note:** git stores the symlink with mode `120000`. On Windows checkouts it works only
> with `git config --global core.symlinks true` and Developer Mode (or admin) enabled — otherwise
> `CLAUDE.md` appears as a plain text file containing the string `AGENTS.md`. Replace the symlink
> with a copy + pre-commit sync hook if that's a problem for your team.

## Claude Code integration

- `AGENTS.md` / `CLAUDE.md` load automatically as project context for every Claude Code session.
- `CLAUDE.local.md` / `AGENTS.local.md` (git-ignored) hold personal overrides.
- `.claude/settings.json` — project-shared settings (permissions, hooks), checked in.
- `.claude/settings.local.json` — personal overrides, git-ignored. See the `.example` file.
- `.claude/commands/` — project-scoped slash commands.
- `.claude/hooks/` — lifecycle scripts (`session-start-context`, `stop-check`).
- `.claude/agents/` — project-scoped sub-agents (none ship by default; built-in `Explore` and
  `Plan` cover most cases).
- `.github/pull_request_template.md` — aligns PR descriptions with the verification checklist
  Claude follows.

See [`docs/references.md`](./docs/references.md) for upstream Claude Code documentation and the
patterns this harness deliberately adopts or rejects.

## Related

- [Anthropic — Claude Code docs](https://code.claude.com)
- [AGENTS.md cross-tool spec](https://agents.md)
- [Trail of Bits — sandboxed Claude Code devcontainer](https://github.com/trailofbits/claude-code-devcontainer)
  (heavier alternative if you need Docker isolation)

## Contributing

Issues and PRs are welcome — see [`CONTRIBUTING.md`](./CONTRIBUTING.md). For security reports,
see [`SECURITY.md`](./SECURITY.md).

## License

[MIT](./LICENSE)
