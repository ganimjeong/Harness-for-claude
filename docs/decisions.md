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

## 2026-05-18: Second-pass adoptions (cross-tool + security)

After a second research pass focused on engineering-team blogs, real OSS
CLAUDE.md files, and published security findings:

- **`AGENTS.md` as canonical, `CLAUDE.md` as symlink** — adopts the cross-tool agent-guide spec used by Codex, Cursor, Aider, and others. Single source of truth, no duplication. Windows symlink caveat documented in `README.md`.
- **Deny command-substitution in `Bash(...)` patterns** — `Bash(*$(*)*)` and the backtick equivalent. Mitigates the published allowlist bypass (CVE-2026-35021) where `$(...)` evaluates before the allow matcher runs.
- **`stop_hook_active` guard in `stop-check`** — prevents the documented stop-hook infinite-loop foot-gun when `scripts/check` fails for reasons Claude cannot fix.
- **Output cap on `session-start-context`** — `head -n 30` final filter. Stdout from `SessionStart` is injected into the session, so it costs context-window tokens every run.
- **`Explore` / `Plan` delegation note in `AGENTS.md`** — points agents at built-in sub-agents for large reads, achieving context isolation without bundling custom sub-agents (still rejected).
- **`additionalDirectories` mention in `docs/workflow.md`** — discoverability for multi-repo work, no config change.
- **`statusLine` example in `.claude/settings.local.json.example`** — opt-in personal preference.
- **OTel env-var stub in `docs/references.md`** — documents observability hooks without enabling them.

Deliberately **not** added (round 2): MCP server entries in baseline settings (project-specific), Spec-Kit/BMAD/Kiro scaffolding (heavyweight; current `tasks/` pattern is enough), Docker / devcontainer harness (imposes Docker on every user; linked from references instead), plugin marketplace presets (personal preference).
