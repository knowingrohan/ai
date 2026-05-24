# Refine Prompt

[![skills.sh](https://skills.sh/b/knowingrohan/refine-prompt)](https://skills.sh/knowingrohan/refine-prompt)

`refine-prompt` is an agent skill for turning rough requests into clear, copy-paste-ready prompts for coding and reasoning agents.

It helps an agent preserve the user's intent, inspect safe local context, identify relevant tools or skills, and return a structured prompt with requirements, execution guidance, output format, and verification criteria.

## Supported Agents

This skill follows the shared `SKILL.md` format and is designed to be agent-agnostic. It should work with agents and CLIs that read agent skills, including Codex, Claude Code, Cursor, GitHub Copilot, Gemini CLI, Windsurf, and other skills-compatible tools.

The repository also includes `skills/refine-prompt/agents/openai.yaml` with lightweight UI metadata for OpenAI-compatible skill listings.

## Install

Install with the skills CLI:

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

```text
skills/
  refine-prompt/
    SKILL.md
    agents/
      openai.yaml
```

## Contributing

Issues and pull requests are welcome. Keep the skill concise, agent-agnostic, and safe for public use. Avoid adding project-specific assumptions, secrets, private paths, or instructions that require unnecessary access to user data.

Before opening a pull request, verify that:

- `skills/refine-prompt/SKILL.md` has valid YAML frontmatter.
- The frontmatter `name` is `refine-prompt`.
- The skill directory name matches the skill name.
- The skill remains useful across agents and repositories.

## License

MIT
