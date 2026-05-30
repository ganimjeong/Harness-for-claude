# Security Policy

## Reporting a vulnerability

If you find a security issue in this harness — for example, a permission rule
that fails to block a class of dangerous command, a hook that can be bypassed,
or a setting that leaks secrets — please report it privately rather than
opening a public issue.

Open a [private security advisory](https://github.com/ganimjeong/Harness-for-claude/security/advisories/new)
on GitHub. Include:

- A description of the issue and its impact.
- A minimal reproduction (commands, settings, or hook input that triggers it).
- The Claude Code version and OS you observed it on, if relevant.

I'll acknowledge within a few days and coordinate a fix and disclosure.

## Scope

In scope:

- `.claude/settings.json` permission rules and deny patterns.
- `.claude/hooks/*` lifecycle scripts.
- `scripts/bootstrap`, `scripts/check`, `scripts/test`.

Out of scope:

- Vulnerabilities in Claude Code itself — report those to Anthropic at
  https://www.anthropic.com/security.
- Vulnerabilities in upstream dependencies — report to the dependency.

## Hardening already in place

See `docs/references.md` for the threat-model decisions baked into this
harness, including the command-substitution deny rules that mitigate
[CVE-2026-35021](https://www.sentinelone.com/vulnerability-database/cve-2026-35021/).
