# Decisions

Record durable project decisions here.

## 2026-05-18: Establish Claude Harness

- Created standard `scripts/bootstrap`, `scripts/check`, and `scripts/test` entrypoints.
- Added `CLAUDE.md` so Claude Code has repository-local operating instructions.
- Added `.claude/settings.json` for shared project settings; `.claude/settings.local.json` is git-ignored for personal overrides.
- Kept the harness language-agnostic until a concrete project stack is introduced.
