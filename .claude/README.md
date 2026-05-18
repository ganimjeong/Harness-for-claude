# .claude

Project-scoped Claude Code configuration.

- `settings.json` — shared, checked-in settings (permissions, hooks).
- `settings.local.json` — personal overrides; git-ignored. See `settings.local.json.example`.
- `commands/` — project slash commands (e.g. `/check`, `/bootstrap`).
- `hooks/` — shell scripts wired into Claude Code lifecycle events via `settings.json`.
- `agents/` — project sub-agents (optional, add as needed).

Default hooks:

- `hooks/session-start-context` — injects branch + uncommitted state + open task notes when interesting.
- `hooks/stop-check` — runs `scripts/check` before session end; blocks with feedback on failure.

See `docs/references.md` for upstream docs links.
