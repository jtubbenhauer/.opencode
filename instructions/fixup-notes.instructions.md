---
description: "Instructions for processing review/fixup notes left by the user in Neovim"
---

# Review Notes (Fixups)

## What This Is

The user leaves review notes while reading code in Neovim. These are stored in `.opencode/fixups.md` in the project root with exact file paths and line numbers.

## When to Check

When the user asks you to fix notes, fixups, review notes, or any variation like:

- "fix the notes" / "fix the fixups" / "fix the review notes"
- "address the notes I left"
- "fix my review comments"

**IMMEDIATELY** read `.opencode/fixups.md` before doing anything else. Do NOT search the codebase for TODOs — the fixups file IS the definitive list.

## File Format

```markdown
# Review Notes

- `path/to/file.ts:42` - description of what to fix
- `path/to/file.ts:15-23` - description for a line range
```

## How to Process

1. Read `.opencode/fixups.md`
2. For each entry:
   a. Read the referenced file at the specified line(s)
   b. Understand the note and apply the fix
   c. After successfully fixing that item, remove that specific line from `.opencode/fixups.md`
3. After all entries are addressed, delete the `.opencode/fixups.md` file entirely

## Important

- Remove entries ONE AT A TIME as you complete them, not all at once
- This way if the session is interrupted, remaining items are preserved
- If a note is unclear, ask the user rather than guessing
- **Only address the fixup notes.** The user may have made other edits to the files during their review (renaming variables, adjusting logic, deleting code, etc.). Those are intentional human changes — do NOT revert or undo them. Your job is strictly to apply the fixes described in the notes, nothing more.
