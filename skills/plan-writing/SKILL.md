# Plan Writing

## Plan File Location

Plans live at `.opencode/plans/<name>-plan.md` in the project root.

## When to Write a Plan

Write a plan file when:

- Scope touches more than 1 file
- Task has more than 3 distinct steps
- User explicitly asks

Otherwise, discuss in chat.

## File Naming

- Feature plans: `feature-name-plan.md` (kebab-case)
- Session summaries: `session-summary-YYYY-MM-DD.md` (in `.opencode/`, not `plans/`)

## Structure

```markdown
# [Feature/Task Name]

## Context

- Problem description
- ClickUp task link if available
- PR link if exists

## Plan

- High-level approach
- Key decisions and rationale

## Progress

- [ ] Task 1 - description
- [x] Task 2 - completed

## Files Changed

- `path/to/file.ts` - what changed

## Notes

- Trade-offs, testing considerations
```

## Rules

- One plan per feature/task
- Update as work progresses
- Delete when complete and merged
- Use markdown checkboxes for progress tracking
- Keep language concise — these may be copied to ClickUp
