# knowingrohan/ai

A monorepo of agent-agnostic skills, hooks, and plugins for Claude Code, Codex, Gemini CLI, Devin, and more.

All skills follow the shared `SKILL.md` format and work with any agent that reads skill files.

## Table of Contents

- [Skills](#skills)
  - [refine-prompt](#refine-prompt)
  - [context-handoff](#context-handoff)
- [Agent Compatibility](#agent-compatibility)
- [Install](#install)
- [Everyday Workflow](#everyday-workflow)
- [Repository Layout](#repository-layout)
- [Contributing](#contributing)
- [License](#license)

---

## Skills

### refine-prompt

[![skills.sh](https://skills.sh/b/knowingrohan/refine-prompt)](https://skills.sh/knowingrohan/refine-prompt)

Turns a rough user request into a clear, copy-paste-ready prompt for a coding or reasoning agent.

Rough prompts produce rough results. When you fire a vague request at an agent, it makes assumptions, skips verification steps, and misses context sitting right in your repo. `refine-prompt` adds a role, concrete requirements, suggested skills, and acceptance criteria — before any code is written.

**Invoke:** `/refine-prompt` (Claude Code) · `skill: refine-prompt` (other agents)

---

### context-handoff

Generates a structured hand-off note before a session ends or context switches to a different agent or collaborator.

Captures what was done, what is still pending, key decisions made, and files changed — then produces a ready-to-paste resume prompt so the next session starts without cold-start friction.

**Invoke:** `/context-handoff` (Claude Code) · `skill: context-handoff` (other agents)

---

## Agent Compatibility

Both skills follow the shared `SKILL.md` format and work with any agent that reads skill files:

| Agent | Reads From |
|---|---|
| Claude Code | `~/.claude/skills/` or `.claude/skills/` |
| Codex | `.codex/skills/` or `.agents/skills/` |
| Gemini CLI | `~/.gemini/skills/` or project-local skills dir |
| Cursor / Windsurf | Project `.agents/skills/` |
| GitHub Copilot | Project-local agent instructions |
| Devin | Uploaded skill files or project docs |
| Anti Gravity / Pi | `.pi/skills/` or equivalent config dir |

## Install

Install a specific skill for the current project:

```sh
npx skills add knowingrohan/ai --skill refine-prompt
npx skills add knowingrohan/ai --skill context-handoff
```

Install globally for all supported local projects:

```sh
npx skills add knowingrohan/ai --skill refine-prompt -g
npx skills add knowingrohan/ai --skill context-handoff -g
```

List all available skills in this repository:

```sh
npx skills add knowingrohan/ai --list
```

## Everyday Workflow

### refine-prompt

**Claude Code** — invoke before a complex task to prevent scope creep:

```text
/refine-prompt
Add a Supabase migration for the new expense-sharing feature in the Flutter app.
```

**Claude Code** — use it to structure a client deliverable before writing a spec:

```text
/refine-prompt
Write a quotation document for a three-tier EdTech platform — frontend, backend, mobile.
```

**Codex** — pass the skill file directly:

```text
Use the refine-prompt skill. I need to add dark mode support to the Next.js site deployed on Vercel.
```

**Gemini CLI** — activate then describe your task:

```text
activate_skill refine-prompt

I want to redesign the navbar. It needs a new logo placement and a mobile hamburger menu.
```

**Devin / Cursor / Windsurf:**

```text
Using the refine-prompt skill from .agents/skills/, refine this into a full implementation prompt:
Scaffold a residential society website — community board, event calendar, resident directory.
```

---

### context-handoff

**Claude Code** — run before ending a long session:

```text
/context-handoff
```

**Claude Code** — run before switching from Claude to Codex mid-task:

```text
/context-handoff
I'm switching to Codex to finish the Flutter auth flow. Generate a hand-off note.
```

**Any agent** — paste the generated resume prompt into the next session's first message to resume without cold-start friction.

---

## Repository Layout

```text
skills/
  refine-prompt/       ← turn rough prompts into structured agent prompts
    SKILL.md
    agents/
      openai.yaml
  context-handoff/     ← generate structured hand-off notes between sessions
    SKILL.md

hooks/                 ← PreToolUse / PostToolUse / Stop automation scripts
  README.md

plugins/               ← MCP servers and agent runtime extensions
  README.md
```

### Adding a Skill

1. Create `skills/<skill-name>/`
2. Add `SKILL.md` with valid YAML frontmatter — `name` must match the directory name:
   ```yaml
   ---
   name: skill-name
   description: One sentence description for agent auto-discovery.
   license: MIT
   ---
   ```
3. Optionally add `agents/openai.yaml` for OpenAI-compatible UI metadata.
4. Add a section for the skill in this README.

### Adding a Hook

1. Create `hooks/<hook-name>/`
2. Add `hook.json` (matcher, command, description) and a `README.md`.
3. See `hooks/README.md` for the full convention.

### Adding a Plugin

1. Create `plugins/<plugin-name>/`
2. Add a `README.md` with install steps and a list of exposed tools or capabilities.
3. See `plugins/README.md` for the full convention.

## Contributing

Issues and pull requests are welcome. Keep skills concise, agent-agnostic, and safe for public use. Avoid project-specific assumptions, secrets, private paths, or instructions that require unnecessary access to user data.

Before opening a pull request, verify that:

- `SKILL.md` has valid YAML frontmatter.
- The frontmatter `name` matches the directory name.
- The skill, hook, or plugin works across agents and repositories.

## License

MIT
