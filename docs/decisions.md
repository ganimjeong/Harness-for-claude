# Decisions

Record durable project decisions here.

## 2026-05-18: Establish Claude Harness

- Created standard `scripts/bootstrap`, `scripts/check`, and `scripts/test` entrypoints.
- Added `CLAUDE.md` so Claude Code has repository-local operating instructions.
- Added `.claude/settings.json` for shared project settings; `.claude/settings.local.json` is git-ignored for personal overrides.
- Kept the harness language-agnostic until a concrete project stack is introduced.

## 2026-05-18: Adopt upstream Claude Code patterns

Adopted patterns from Anthropic's official Claude Code best-practices and hooks
references (see `docs/references.md`):

- **`Stop` hook running `scripts/check`** — implements Anthropic's "give Claude a way to verify its work" recommendation. Blocks session end on failure so issues are surfaced, not hidden. Toggle to advisory by changing `exit 2` to `exit 0` in `.claude/hooks/stop-check`.
- **`SessionStart` hook for context** — emits branch, uncommitted changes, and open task notes only when state is non-trivial. Keeps the session preamble noiseless on a clean repo.
- **`deny` rules in `settings.json`** — explicitly block reads of `.env*` and `secrets/**`, and block `git push --force*`. Pairs with a narrow `allow` list rather than a wide-open one.
- **`CLAUDE.local.md` convention** — git-ignored personal overrides, per Anthropic's documented pattern.
- **`.github/pull_request_template.md`** — mirrors the verification checklist in `CLAUDE.md` so human PRs follow the same shape Claude does.
- **`docs/references.md`** — durable pointers to upstream docs, plus notes on which patterns we deliberately rejected (bloated CLAUDE.md, many default sub-agents, wide Bash allowlists).

Deliberately **not** added: default sub-agents (stock agents cover the common cases), `/commit` and `/pr` slash commands (built-in Claude Code behavior already handles these well).
