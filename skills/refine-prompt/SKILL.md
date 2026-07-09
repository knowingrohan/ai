---
name: refine-prompt
description: Transform a user's raw prompt into a clear, expert-level prompt for an AI coding or reasoning agent. Use when the user asks to refine, improve, rewrite, or engineer a prompt, especially when they want the output to leverage project context, agent memories, local instruction files, or available skills.
license: MIT
---

# Refine Prompt

Use this skill to turn a rough user prompt into a copy-paste-ready prompt that another AI agent can execute well.

The refined prompt should preserve the user's intent, clarify missing structure, and use available project context without inventing facts.

## Context Discovery

Before writing the refined prompt, inspect the current agent and project context that is safely available.

Prefer local context over assumptions:

- Codex: check active instructions, loaded skills, repository files such as `AGENTS.md`, `.codex/`, `.agents/`, `README.md`, and nearby project docs.
- Claude: check `CLAUDE.md`, project memories, repository docs, and available skills or commands.
- Gemini CLI: check `GEMINI.md`, repository docs, configured memories, and available tools.
- Other agents: check their local instruction files, memory files, tool registry, and project documentation when available.

Use fast, scoped search. Do not scan huge dependency or build folders unless the user explicitly asks.

If context cannot be inspected, say that the prompt was refined from the provided text only.

## Prompt Refinement Workflow

1. Identify the user's real task from the raw prompt — the larger goal, who it is for, and what the output enables, not just the literal ask.
2. Select the best role for the agent, such as `Senior Software Engineer`, `QA Engineer`, `Product Manager`, `Technical Writer`, `UX Designer`, `Security Reviewer`, or `Data Analyst`.
3. Extract constraints, success criteria, expected output format, files, technologies, risks, and required verification steps.
4. Identify useful local or global skills, commands, tools, memories, or project docs. Mark these as suggestions, not requirements, unless the user explicitly requires them.
5. Fill the prompt anatomy below. The refined prompt is a session contract that sets full context and working style for a fresh session — not a step checklist.
6. Preserve uncertainty. Use phrases like "inspect and confirm" when the raw prompt lacks details.

## Output Format

Return a short lead-in followed by one fenced text block containing the refined prompt.

After the fenced block, include a concise optional note only if context was unavailable or assumptions were important.

Do not include long explanations about prompt engineering.

### Prompt Anatomy

The block is built from these sections, in this order. Each section is one tight paragraph or a short bullet run.

| Section | Job | When to include |
| --- | --- | --- |
| Role | The expert the agent should be | Always |
| Task | The goal + the why, not the steps | Always |
| Context | What the project knows, the chat forgets | Always |
| Skill | Saved once, loaded in one word | When a relevant skill, command, or quality reference exists |
| Effort | Don't let it undersell itself | When the problem is non-routine |
| Act | Stop it from overplanning | Always |
| Scope | The simplest thing that works | Always |
| Delegate | Subagents do the boring work | When independent, parallelizable subtasks exist |
| Evidence | Audit before reporting | When the output makes claims that can be verified in-session |
| Memory | Promote lessons into the project | When working inside a persistent project |
| Checkpoint | Pause only when it must | Always |
| Report | The TLDR comes first | Always |

Sections marked Always appear in every refined prompt, even for small tasks. Omit a situational section entirely when it does not apply — do not include it with filler.

Use this structure inside the copy-paste block:

```text
Role: <specific expert role>

Task: I'm working on <larger goal> for <who it's for>. They need <what the output enables>. With that in mind: <the specific task>.
- <concrete requirement from the raw prompt>
- <inferred requirement that is safe and useful>

Context: Use these first — <specific local files, memories, docs, tools, or "inspect available project context first">. If something you need is not listed, inspect the project before acting. Don't guess.

Skill: /<skill-name> — apply it fully. <one line on why it fits, or a pasted reference of what "good" looks like>

Effort: This is a <routine | hard | hardest-unsolved> problem. Don't undersell yourself — scope it like it's at the top of your range.

Act: When you have enough information to act, act. Don't re-derive what's established, re-litigate decisions already made, or survey options you won't pursue. Weighing a choice? Give a recommendation.

Scope: Do the simplest thing that works well. No extra features, refactors, or abstractions. No error handling for scenarios that can't happen. <If the raw prompt describes a problem rather than requesting a change: "The deliverable is your assessment — do not apply fixes until asked.">

Delegate: Split independent subtasks across subagents and keep working. Verify the result against this prompt with a fresh-context check before finishing.

Evidence: Before reporting, audit every claim against a real result from this session — a command output, a file, a test run. Unverified? Say so.

Memory: If you learn something durable about this project or user that will matter next time, state it at the end so it can be promoted into the project's instructions.

Checkpoint: Pause only for destructive actions, real scope changes, or input only the user can provide. Otherwise proceed end to end. Never end your turn on a promise.

Report: Open with the outcome — the TLDR the user would ask for. <exact deliverable format, if the user specified one.> Complete sentences, no arrow chains, no shorthand the user never saw. Clear beats short.
```

## Post-Refinement Flow

After presenting the refined prompt in an interactive session, run these steps in order. In non-interactive or pipeline contexts, present the prompt and stop.

1. Copy the refined prompt to the system clipboard — the content of the fenced block only, without the fences. Write it to a temp file first, then pipe the file to the first available clipboard tool (this avoids shell-escaping issues):
   - macOS: `pbcopy < file`
   - Linux (Wayland): `wl-copy < file`; (X11): `xclip -selection clipboard < file`
   - Windows / WSL: `clip.exe < file`
2. Confirm the copy with one line: `Prompt copied to clipboard.` If no clipboard tool or shell access is available, say `Could not copy to clipboard — copy the block above manually.` and continue.
3. Ask the user how to proceed. Use the agent's native question tool when one exists (for example `AskUserQuestion` in Claude Code); otherwise ask as plain text. Present all three of these as explicit labeled options — do not fold the third into the tool's automatic free-form fallback:
   - **Execute** — run the refined prompt now, in this session, as if the user had pasted it.
   - **Modify** — the user edits the copied prompt outside the session and pastes the final version back; execute the pasted prompt verbatim instead of the original.
   - **Something else** — the user states what they want next; follow that instruction.
4. Do not execute anything until the user has answered.

## Quality Bar

The refined prompt must be:

- Clear enough for a different agent to act on without seeing the raw prompt.
- Faithful to the user's request.
- Specific about role, task, context, deliverables, and verification.
- Practical for agent CLIs such as Codex, Claude, Gemini CLI, Cursor, and similar tools.
- Written as an instruction to the executing agent, not as commentary about the user.

## Skill Suggestions

When filling the `Skill:` section, draw from:

- Skills already visible in the current agent context.
- Project-local skills discovered in directories such as `.agents/skills`, `.codex/skills`, `.claude/skills`, or similar.
- Global skills only when known or discoverable.

If no relevant skill is found but skill discovery matters for the task, use:

```text
Skill: None found from available context — inspect project-local and global skills before starting. Here's what "good" looks like: <short quality reference>.
```

Otherwise omit the `Skill:` section.

## Handling Weak Raw Prompts

If the raw prompt is vague, still produce a useful refined prompt. Add an `Assumptions To Confirm` section inside the copy-paste block only when those assumptions materially affect execution.

Do not ask the user for clarification unless the raw prompt is impossible to refine safely.
