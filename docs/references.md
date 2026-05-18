# References

Authoritative sources behind the conventions in this harness. When in doubt,
prefer these over secondary blog posts.

## Claude Code (official)

- [Claude Code best practices](https://code.claude.com/docs/en/best-practices) — what to put in CLAUDE.md, plan-then-code flow, self-verification.
- [Hooks reference](https://code.claude.com/docs/en/hooks) — `SessionStart`, `PreToolUse`, `PostToolUse`, `Stop` events; exit-code semantics.
- [Configure permissions](https://code.claude.com/docs/en/permissions) — `allow` / `deny` rule syntax, gitignore-style file patterns, `Bash(...)` matching rules.
- [Sub-agents](https://code.claude.com/docs/en/sub-agents) — `.claude/agents/<name>.md` format with YAML frontmatter.
- [Slash commands](https://code.claude.com/docs/en/slash-commands) — `.claude/commands/<name>.md` format.
- [Settings reference](https://code.claude.com/docs/en/settings) — full `settings.json` schema.

## Patterns we deliberately adopted

- **AGENTS.md + CLAUDE.md symlink** — single canonical agent guide following the [cross-tool AGENTS.md spec](https://agents.md). Edit `AGENTS.md`; `CLAUDE.md` is a symlink.
- **Stop hook for self-verification** — Anthropic's "give Claude a way to verify its work" recommendation, implemented as `.claude/hooks/stop-check` running `scripts/check`. Guards against [stop-hook infinite loops](https://dev.to/yurukusa/5-claude-code-hook-mistakes-that-silently-break-your-safety-net-58l3) by checking `stop_hook_active`.
- **SessionStart context loader** — emits branch + uncommitted changes + task notes only when state is interesting, output capped at 30 lines so it can't bloat the context window.
- **Deny rules including command-substitution patterns** — narrow `allow` list plus explicit `deny` for `.env*`, `secrets/`, force-push, and `Bash(*$(*)*)` / `Bash(*` + backtick + `*)` to mitigate the [allowlist command-substitution bypass](https://mfyz.com/claude-code-allowlist-command-substitution-bypass/) ([CVE-2026-35021](https://www.sentinelone.com/vulnerability-database/cve-2026-35021/)).
- **CLAUDE.local.md / AGENTS.local.md** — official override convention for personal notes that should not be checked in.
- **Delegate to built-in `Explore` / `Plan` sub-agents** for large reads or design questions — keeps the main context clean without bundling custom sub-agents.

## Patterns we deliberately rejected

- **Bloated CLAUDE.md.** Anthropic warns this causes Claude to ignore instructions. We keep `AGENTS.md` under ~100 lines.
- **Many default sub-agents.** Stock agents (general-purpose, Explore, Plan) already cover most needs; we ship none by default and add only when a project needs one.
- **Wide Bash allowlists / argument-pattern allows.** Argument matching is fragile (variables, redirects, wrappers bypass it); we prefer narrow command prefixes plus a strong deny list.
- **MCP servers in baseline settings.** Universally useful ones (filesystem, sequential-thinking, git) carry install cost and project-specific config — leave to per-project.
- **Bundled spec-driven dev frameworks (Spec-Kit, BMAD, Kiro).** The minimal `specs/` + `tasks/TEMPLATE.md` pattern covers ~80% of the value without the framework overhead.
- **Devcontainer / Docker harness.** Trail of Bits' [sandboxed devcontainer](https://github.com/trailofbits/claude-code-devcontainer) is excellent but imposes Docker on every user; link it from here instead of shipping it.

## Observability (optional)

Claude Code has built-in OpenTelemetry support for token/cost tracking. The
harness does not enable it by default. To turn it on in your shell or
`.claude/settings.local.json` `env` block:

```
CLAUDE_CODE_ENABLE_TELEMETRY=1
OTEL_METRICS_EXPORTER=otlp        # or "console" for local debugging
OTEL_EXPORTER_OTLP_ENDPOINT=...   # your OTel collector
```

See [Claude Code observability docs](https://code.claude.com/docs/en/agent-sdk/observability) for the full set of supported env vars.
