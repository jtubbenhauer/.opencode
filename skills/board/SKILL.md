---
name: board
description: Manage personal kanban boards. Triggered by "add todo", "doing X", "done with X", "what's on my board", "move X to doing", "board", "todo X".
---

# Board

Manage kanban-style boards at `~/dev/quartz/content/`.

## Boards

- **Work**: `~/dev/quartz/content/work/board.md` — three columns: Todo, Doing, Done
- **Personal**: `~/dev/quartz/content/personal/board.md` — single column: Todo

## Trigger Phrases

- "add todo X", "todo X" — add item to Todo
- "doing X", "started X", "working on X" — move/add item to Doing (work only)
- "done with X", "finished X", "completed X" — move item to Done (work) or remove (personal)
- "what's on my board", "show board", "board" — read and summarise current state
- "move X to doing/todo/done" — explicit column move

## Behaviour

1. **Detect which board** from context:
   - In a work project / discussing work → work board
   - Personal context → personal board
   - If ambiguous, ask

2. **Read the board file**, find the relevant section heading (`## Todo`, `## Doing`, `## Done`)

3. **Apply the change**:
   - **Add**: append `- item text` under the target section
   - **Move**: remove from current section, append under target section
   - **Done (work)**: move to Done with date stamp: `- ~~item text~~ (YYYY-MM-DD)`
   - **Done (personal)**: remove the item entirely (it's done, no need to track)
   - **Show**: read the file and summarise what's in each column

4. **Write back** the updated file

## Format Rules

- Items are markdown list items: `- text`
- Done items (work board): `- ~~item text~~ (YYYY-MM-DD)`
- Keep items short — one line each, no sub-bullets
- No priority markers or labels — keep it flat and simple
- Done section: keep last 2 weeks of items, archive older ones by removing them

## Work Board Structure

```markdown
## Doing

- Active task one
- Active task two

## Todo

- Upcoming task

## Done

- ~~Completed task~~ (2026-05-07)
```

## Personal Board Structure

```markdown
## Todo

- Thing to do
- Another thing
```

## Edge Cases

- If the user says "done with X" but X isn't on the board, just acknowledge it's done (don't error)
- If moving to Doing and item isn't in Todo, add it fresh to Doing
- Fuzzy match item text — "done with patient search" matches "- Patient search refactor"

## Permissions

This skill MAY:
- Add, move, and remove items on either board
- Archive old done items (>2 weeks)

This skill must NEVER:
- Push to git (the systemd timer handles that)
- Modify note files (that's the note-capture skill's job)
