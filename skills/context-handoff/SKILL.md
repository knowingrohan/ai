---
name: context-handoff
description: Before ending a session or switching agents, produce a structured hand-off note — what was done, what is pending, key decisions made, files changed, and a ready-to-paste resume prompt for the next session or agent. Use when the user says they are wrapping up, switching tools, handing off to a teammate, or starting a new session on the same project.
license: MIT
---

# Context Handoff

Use this skill to generate a structured hand-off note before a session ends or context switches to a different agent or collaborator.

It captures what happened, what is still open, and produces a copy-paste resume prompt so the next session starts without cold-start friction.

## When to Use

- Before closing a long coding session
- Before switching from one agent to another (Claude → Codex, Codex → Devin, etc.)
- Before handing a branch to a teammate
- After a context window warning — capture state before compression loses it
- At a natural checkpoint between phases (research done, implementation next)

## Context Discovery

Before generating the hand-off, collect available session state:

- Check the current working directory, open files, and recent edits
- Check git status and recent commit messages (`git log --oneline -10`, `git diff HEAD`)
- Check todo lists, task trackers, or in-progress notes if available
- Check agent memories, loaded skills, and any `CLAUDE.md`, `AGENTS.md`, or `GEMINI.md` in scope
- Note any failing tests, open errors, or unresolved decisions from the session

Use what is safely readable. Do not scan large build or dependency folders.

## Hand-Off Generation Workflow

1. Summarize what was completed this session in 3–7 bullet points.
2. List what is still pending — incomplete tasks, blocked items, follow-ups.
3. Note key decisions made and the reasoning behind them (especially non-obvious ones).
4. List files that were created or significantly modified.
5. Flag any open risks, known issues, or things the next agent should be careful about.
6. Write a ready-to-paste resume prompt the next agent can use to pick up exactly where this session left off.

## Output Format

Return a short lead-in, then one fenced markdown block containing the hand-off note, followed by a second fenced text block containing the resume prompt.

### Hand-off note structure

```markdown
## Session Hand-off — [Project Name] — [Date]

### Completed
- <what was done, specific and concrete>

### Pending
- <tasks still open, in priority order>

### Key Decisions
- <decision>: <brief reason>

### Files Changed
- `path/to/file` — <what changed>

### Watch Out For
- <risks, known issues, or gotchas the next agent should know>
```

### Resume prompt structure

```text
Role: <same role as the current session, or the appropriate next role>

Context: Resuming work on [project]. Last session ended with:
- <1–3 sentence summary of state>

Pending tasks (in order):
1. <next concrete task>
2. <task after that>

Key context:
- <critical decision or constraint the new session must know>
- <file or system state the new agent should check first>

Start by confirming the current state of [most important file or system], then proceed with task 1.
```

## Quality Bar

The hand-off note must be:

- Concrete enough that someone who was not in this session can continue without asking questions.
- Short enough to paste into a new session's first message without overwhelming context.
- Faithful to what actually happened — do not invent completed work.
- Actionable — the next task should be clear from the resume prompt alone.

## Handling Incomplete Information

If session history is not fully available (context was compressed, agent memory was not loaded), produce the best hand-off from what is visible and add a note:

```text
Note: Session history was partially available. Verify current file state before resuming.
```

Do not fabricate completed tasks or invented file paths.
