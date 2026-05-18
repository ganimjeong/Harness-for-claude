# Workflow

## Starting Work

Run:

```sh
scripts/bootstrap
```

Then inspect the repository and confirm the intended change.

## During Work

- Keep task notes in `tasks/` when context would otherwise be lost.
- Prefer small, reviewable commits.
- Update `docs/decisions.md` for decisions future agents should preserve.

## Finishing Work

Run:

```sh
scripts/check
```

If `scripts/check` cannot run, document why in the final handoff.

## Multi-Repo Work

If a task crosses into a sibling repository, prefer adding that repo to your
local `.claude/settings.local.json` under `additionalDirectories` (or pass
`--add-dir` on the CLI) rather than juggling separate sessions. The harness
intentionally ships an empty `additionalDirectories` so cross-repo scope is a
deliberate, per-user opt-in.
