---
name: plan-writing
description: Conventions for writing plan documents to .opencode/plans/. Covers when to write a plan, file naming, required structure (Context/Plan/Progress/Files Changed/Notes), and update rules. Load when creating or updating plan files.
---

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

## Scaffold Handoff

When a plan will be handed off for scaffolding (stub-first implementation), include these sections:

### Architecture Constraints

State as hard rules, not suggestions. Examples:

- "Translation maps are stored in PostgreSQL, not Firestore"
- "GCS folder structure: pending/{migrationId}-{syncId}/"
- "The validate function reads from pending/ but MUST move files to processed/ after validation"

### Lint Contract

If the project has strict linting, state what must pass:

- "Must pass eslint --max-warnings 0"
- "No @typescript-eslint/no-unsafe-* violations"
- "No @typescript-eslint/no-explicit-any violations"

### Anti-Patterns for This Feature

Feature-specific things to avoid, beyond the global anti-patterns instruction:

- "Do not define ITranslationMapGcs — use the existing ITranslationMap type and transform with serialise()"
- "Do not add _deleteOrphanedRows yet — that's a future task"
- "Do not create a new database connection utility — use getDatabaseConnection() from db-utils"

### File Classification

List which files fall into each category:

- **Write in full**: types, interfaces, enums, constants, barrel exports, configs (anything structural with no business logic)
- **Stub with TODOs**: services, handlers, utilities (anything with method bodies containing business logic)
- **Do not create**: files that might seem related but aren't needed for this feature

## Rules

- One plan per feature/task
- Update as work progresses
- Delete when complete and merged
- Use markdown checkboxes for progress tracking
- Keep language concise — these may be copied to ClickUp
