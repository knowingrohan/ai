# Hooks

Agent automation hooks — `PreToolUse`, `PostToolUse`, and `Stop` scripts that run in response to agent events.

## Directory Convention

Each hook lives in its own directory:

```
hooks/
  <hook-name>/
    hook.json       ← hook config (matcher, command, description)
    README.md       ← what it does, when to use it, install steps
```

## Install

Copy a hook config into your agent settings:

- **Claude Code**: add the hook object to `~/.claude/settings.json` under `hooks`
- **Codex**: add to `.codex/settings.json`
- **Other agents**: check your agent's hooks documentation

## Adding a Hook

1. Create `hooks/<hook-name>/`
2. Add `hook.json` with `matcher`, `command`, and `description` fields
3. Add a `README.md` with install and usage instructions
