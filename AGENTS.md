# Agent Harness Instructions

Canonical agent guide for this repository. Shared as `AGENTS.md` (the
cross-tool spec used by Codex, Cursor, Aider, etc.) and `CLAUDE.md` (a
symlink to this file) so all coding agents read the same source of truth.
Edit `AGENTS.md`; never edit `CLAUDE.md` directly.

The repository is a baseline workspace for AI-agent-driven implementation,
intentionally language-agnostic — extend it as a concrete stack is added.

## Adopting this harness

After cloning or using "Use this template" to start a new project from this
repo, replace the artifacts that describe *this* harness (not your project):

1. **`LICENSE`** — update the copyright line (`(c) 2026 ganimjeong`) to your
   own name. Keep MIT or switch to your preferred license.
2. **`README.md`** — rewrite for your project. The shipped README is about
   the harness itself.
3. **`SECURITY.md`** — update the GitHub security advisory URL to point at
   your repo.
4. **`.github/social-preview.png`** and **`.github/social-preview.svg`** —
   regenerate or delete. The PNG is generated from the SVG via
   `qlmanage -t -s 1280 -o .github/ .github/social-preview.svg` on macOS;
   center-crop the 1280×1280 output to 1280×640 with
   `sips --cropToHeightWidth 640 1280 input.png --out output.png`.
5. **`.github/ISSUE_TEMPLATE/`** — keep, remove, or tailor to your project's
   triage flow.
6. **`CONTRIBUTING.md`** — the shipped version describes harness scope rules
   ("no Docker, no bundled MCP"); replace with rules that fit your project.

Everything in `.claude/`, `scripts/`, `docs/decisions.md`, `tasks/TEMPLATE.md`,
and the `AGENTS.md` operating principles is intended to be kept as-is and
extended. That's the actual harness.

## Operating Principles

- Read the repository before editing. Prefer existing conventions over new ones.
- Keep changes scoped to the requested task. Do not add features, refactors, or
  abstractions that were not asked for.
- Do not revert user changes unless explicitly asked.
- Use `rg` for searching when available.
- Use `scripts/bootstrap` before first development when dependencies are present.
- Use `scripts/check` before handing work back.
- For large search sweeps or multi-file research, delegate to a sub-agent
  (Claude Code's built-in `Explore` for read-only search, `Plan` for design
  questions) so the main session keeps a clean context.
- Add or update tests when behavior changes.
- Document decisions that affect future work in `docs/decisions.md`.

## Repository Layout

- `AGENTS.md`: This file. Canonical agent instructions.
- `CLAUDE.md`: Symlink to `AGENTS.md`. Claude Code auto-loads this.
- `CLAUDE.local.md` / `AGENTS.local.md`: Optional personal overrides (git-ignored).
- `README.md`: Human-facing overview and workflow.
- `LICENSE`: MIT license.
- `CONTRIBUTING.md`: Contribution workflow and scope rules.
- `SECURITY.md`: Vulnerability reporting policy.
- `docs/`: Project notes, decisions, and reference links.
- `scripts/`: Stable automation entrypoints (`bootstrap`, `check`, `test`).
- `tasks/`: Task briefs and working notes — start from `tasks/TEMPLATE.md`.
- `.claude/`: Claude Code project configuration.
  - `settings.json`: Shared, checked-in settings (permissions, hooks).
  - `settings.local.json`: Personal overrides (git-ignored).
  - `commands/`: Project slash commands.
  - `hooks/`: Lifecycle scripts (session start, stop, etc.).
  - `agents/`: Project sub-agents.
- `.github/`: Issue templates, PR template, CI workflow.

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
   The `Stop` hook also runs `scripts/check` automatically and will block the
   session end if it fails — so failures get surfaced and fixed, not hidden.
3. Report changed files and any verification that could not be run.

## Further Reading

See `docs/references.md` for links to Anthropic's official Claude Code docs
(hooks, permissions, sub-agents, settings).
