---
description: "Instructions for maintaining plan documentation in .opencode directory"
---

# Plan Documentation Workflow

## Permissions

**Plan mode exception:** Even when in read-only Plan mode, you are ALWAYS permitted to write to the `.opencode/` directory without asking permission. This is explicitly allowed because:

- Plan documentation is part of the planning process, not execution
- The `.opencode/` folder exists specifically for session notes and planning artifacts
- Documenting plans should never be blocked by read-only restrictions

You do NOT need to ask permission to create or update files in `.opencode/`.

## When to Document

You MUST create or update a plan document in the `.opencode/` directory (or `.opencode/plans/` subfolder) when:

1. **Starting a planning phase** - When the user asks you to plan a feature, fix, or refactor
2. **Completing planning** - After discussing and finalizing an implementation plan
3. **Making significant progress** - When completing major steps of an implementation
4. **Session ends** - Before a session ends (if the user asks or if significant work was done)

## Plan Document Structure

Each plan document should be structured for eventual ClickUp documentation:

```markdown
# [Feature/Task Name]

## Context

- What problem are we solving?
- Link to ClickUp task(s) if available
- Link to PR if one exists

## Plan

- High-level approach
- Key decisions made and why

## Progress

- [ ] Task 1 - description
- [x] Task 2 - completed description
- [ ] Task 3 - pending

## Files Changed

- `path/to/file.ts` - brief description of change

## Notes

- Important decisions or trade-offs
- Things to watch out for
- Testing considerations
```

## Maintenance Rules

1. **Keep it current** - Update the plan as work progresses
2. **Compress completed work** - Move detailed implementation notes to a "Completed" section or remove if no longer relevant
3. **Remove outdated content** - Delete plans that are no longer applicable
4. **One plan per feature/task** - Don't mix unrelated work in the same document

## File Naming

- Use descriptive kebab-case names: `feature-name-plan.md`
- For session summaries: `session-summary-YYYY-MM-DD.md`
- Place feature plans in `.opencode/plans/`
- Place session summaries in `.opencode/`

## Proactive Documentation

You should NOT wait to be asked. When in a planning phase:

1. Create the plan document early
2. Update it as decisions are made
3. Mark tasks complete as they finish

If the user asks "what did we do" or "where were we", the `.opencode/` folder should have the answer.

## Format for ClickUp

When documenting plans, keep in mind they may be copied to ClickUp tasks:

- Use clear markdown formatting
- Include task lists with checkboxes
- Add code references with file paths and line numbers where applicable
- Keep language professional but concise
