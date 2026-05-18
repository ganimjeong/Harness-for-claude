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

- **Stop hook for self-verification** — Anthropic's "give Claude a way to verify its work" recommendation, implemented as `.claude/hooks/stop-check` running `scripts/check`.
- **SessionStart context loader** — emits branch + uncommitted changes + task notes only when state is interesting, so the preamble stays noiseless.
- **Deny rules over wide-open allows** — narrow `allow` list plus explicit `deny` for `.env`, `secrets/`, and force-push.
- **CLAUDE.local.md** — official override convention for personal notes that should not be checked in.

## Patterns we deliberately rejected

- **Bloated CLAUDE.md.** Anthropic warns this causes Claude to ignore instructions. We keep CLAUDE.md under ~100 lines.
- **Many default sub-agents.** Stock agents (general-purpose, Explore, Plan) already cover most needs; we ship none by default and add only when a project needs one.
- **Wide Bash allowlists / argument-pattern allows.** Argument matching is fragile (variables, redirects, wrappers bypass it); we prefer narrow command prefixes plus a strong deny list.
