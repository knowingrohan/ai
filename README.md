# Refine Prompt

[![skills.sh](https://skills.sh/b/knowingrohan/refine-prompt)](https://skills.sh/knowingrohan/refine-prompt)

`refine-prompt` is an agent skill for turning rough requests into clear, copy-paste-ready prompts for coding and reasoning agents.

It helps an agent preserve the user's intent, inspect safe local context, identify relevant tools or skills, and return a structured prompt with requirements, execution guidance, output format, and verification criteria.

## Why refine-prompt?

Rough prompts produce rough results. When you fire a vague request at a coding agent, it makes assumptions, skips verification steps, and misses context sitting right in your repo. `refine-prompt` turns that rough input into a structured prompt — with a clear role, concrete requirements, suggested skills, and acceptance criteria — before any code is written.

## Supported Agents

This skill follows the shared `SKILL.md` format and is designed to be agent-agnostic. It works with any agent or CLI that reads `SKILL.md` files:

| Agent | Reads From |
|---|---|
| Claude Code | `~/.claude/skills/` or `.claude/skills/` |
| Codex | `.codex/skills/` or `.agents/skills/` |
| Gemini CLI | `~/.gemini/skills/` or project-local skills dir |
| Cursor / Windsurf | Project `.agents/skills/` |
| GitHub Copilot | Project-local agent instructions |
| Devin | Uploaded skill files or project docs |
| Anti Gravity / Pi | `.pi/skills/` or equivalent config dir |

The repository also includes `skills/refine-prompt/agents/openai.yaml` with lightweight UI metadata for OpenAI-compatible skill listings.

## Install

Install for the current project:

```sh
npx skills add knowingrohan/refine-prompt --skill refine-prompt
```

Install globally for all supported local projects:

```sh
npx skills add knowingrohan/refine-prompt --skill refine-prompt -g
```

List available skills in this repository:

```sh
npx skills add knowingrohan/refine-prompt --list
```

## Everyday Workflow

These examples show how `refine-prompt` fits into real development sessions across different agents and project types.

### Claude Code

Invoke directly when you have a rough idea and want a structured prompt before acting:

```text
/refine-prompt
Fix the layout issue on the footer — the icons look misaligned on mobile.
```

Use it before a complex feature to prevent scope creep:

```text
/refine-prompt
Add a Supabase migration for the new expense-sharing feature in the Flutter app.
```

Use it to structure a client deliverable before writing a spec:

```text
/refine-prompt
Write a quotation document for a three-tier EdTech platform — frontend, backend, mobile.
```

### Codex

Pass the skill file directly and describe your task:

```text
Use the refine-prompt skill. I need to add dark mode support to the Next.js site deployed on Vercel.
```

### Gemini CLI

Activate the skill, then describe your task:

```text
activate_skill refine-prompt

I want to redesign the navbar for the KLAOA site. It needs a new logo placement and a mobile hamburger menu.
```

### Devin / Cursor / Windsurf

Reference the skill in your task description:

```text
Using the refine-prompt skill from .agents/skills/, refine this into a full implementation prompt:
Scaffold a new residential society website — community board, event calendar, resident directory.
```

### Anti Gravity / Pi

```text
skill: refine-prompt
task: Add real-time presence indicators to the Flutter chat screen using Supabase Realtime.
```

## Usage Examples

Ask your agent to use the skill when a prompt needs structure:

```text
Use the refine-prompt skill to turn this rough task into a strong prompt for a coding agent:
Fix the flaky auth tests and make sure CI passes.
```

Use it with project context:

```text
Use refine-prompt. Inspect the repo docs and local agent instructions first, then rewrite this into a complete implementation prompt:
Add a release checklist for our npm package.
```

## Repository Layout

This is a monorepo of agent skills. Each skill lives in its own directory under `skills/`.

```text
skills/
  refine-prompt/
    SKILL.md
    agents/
      openai.yaml
  context-handoff/
    SKILL.md
```

### Adding a New Skill

1. Create a directory: `skills/<skill-name>/`
2. Add `SKILL.md` with valid YAML frontmatter — `name` must match the directory name:
   ```yaml
   ---
   name: skill-name
   description: One sentence description for agent auto-discovery.
   license: MIT
   ---
   ```
3. Optionally add `agents/openai.yaml` for OpenAI-compatible UI metadata.
4. Add install instructions and usage examples to the root `README.md`.

Skills in this repo follow the shared `SKILL.md` format and are agent-agnostic by design.

## Contributing

Issues and pull requests are welcome. Keep the skill concise, agent-agnostic, and safe for public use. Avoid adding project-specific assumptions, secrets, private paths, or instructions that require unnecessary access to user data.

Before opening a pull request, verify that:

- `skills/refine-prompt/SKILL.md` has valid YAML frontmatter.
- The frontmatter `name` is `refine-prompt`.
- The skill directory name matches the skill name.
- The skill remains useful across agents and repositories.

## License

MIT
