---
description: "Instructions for processing review/fixup notes left by the user in Neovim"
---

# Review Notes (Fixups)

## What This Is

The user leaves review notes while reading code in Neovim, stored in `.opencode/fixups.md` in the project root with exact file paths and line numbers.

## Trigger Phrases

- "fix the notes" / "fix the fixups" / "fix the review notes"
- "address the notes I left"
- "fix my review comments"

When triggered, IMMEDIATELY read `.opencode/fixups.md`. Do NOT search the codebase for TODOs — the fixups file IS the definitive list.

## File Format

```markdown
# Review Notes

- `path/to/file.ts:42` - description of what to fix
- `path/to/file.ts:15-23` - description for a line range
```

## Processing — Implement Agent

For each entry:

1. Read the referenced file at the specified line(s)
2. Apply the fix in code
3. Remove that line from `.opencode/fixups.md`

After all entries are done, delete `.opencode/fixups.md` entirely.

## Processing — Plan Agent

You can't write code, but you can still address notes through conversation (answering questions, clarifying intent, designing fixes, updating plan docs).

For each entry:

1. Read the referenced file at the specified line(s)
2. Address the note via discussion with the user, or by updating a plan doc in `.opencode/plans/` if the note relates to in-progress planning
3. Once the user confirms it's addressed (or once you've folded it into a plan doc), remove that line from `.opencode/fixups.md`

If a note clearly requires code changes that can only happen in implement mode, leave it in the file and tell the user it needs the implementer.

After all addressable entries are done, delete `.opencode/fixups.md` entirely.

## Universal Rules

- Process entries ONE AT A TIME — interrupted sessions preserve remaining items
- If a note is unclear, ask rather than guess
- **Only address the fixup notes.** The user may have made other intentional edits during review (renames, logic adjustments, deletions). Do NOT revert those.
