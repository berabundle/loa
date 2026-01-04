# Codex + Loa

This repo ships Codex-compatible skills and prompt wrappers so you can use Loa workflows in Codex as well as Claude Code.

## What is included

- Repo-scoped Codex skills under `.codex/skills/`.
- Prompt wrappers under `.codex/prompts/` that map Loa commands to Codex `/prompts:<name>` invocations.

## Setup (Codex)

1. Install prompts to your user Codex directory:

```bash
./scripts/install-codex-prompts.sh
```

2. Start Codex from the repo root (so Codex loads `.codex/skills`):

```bash
codex
```

3. Use Loa commands via Codex prompts (note the `/prompts:` prefix):

```
/prompts:plan-and-analyze
/prompts:architect
/prompts:sprint-plan
/prompts:implement sprint-1
/prompts:review-sprint sprint-1
/prompts:audit-sprint sprint-1
```

4. Optional: run any Loa command via the generic prompt:

```
/prompts:loa COMMAND=ride ARGS="--phase extraction"
```

## Command invocation differences

- Claude Code uses slash commands (e.g. `/implement sprint-1`). Codex uses prompt invocations: `/prompts:implement sprint-1`.
- Positional args work for most commands. Named args also work when the command defines them (e.g. `sprint_id=sprint-1`).
- For `translate`, the wrapper accepts the same "for" phrasing as the Claude command (e.g. `/prompts:translate @doc for execs`).

## How it works

- The `loa-command-runner` skill reads `.claude/commands/<name>.md`, runs pre-flight checks, loads context, and dispatches to the referenced Loa skill.
- Each prompt wrapper calls `loa-command-runner` with the command name and raw arguments.
- The Loa skills remain in `.claude/skills/` and are never edited directly.
- Thin wrapper skills live in `.codex/skills/` because Codex only loads skills from that directory.
- Codex prompt wrappers invoke skills using the `$skill-name` syntax (so `$loa-command-runner` resolves to the `.codex/skills/loa-command-runner` implementation).

## Notes

- Codex custom prompts are user-scoped (`~/.codex/prompts`), so the install script copies repo prompts into your home directory.
- If you update prompts, re-run the install script and restart Codex.
