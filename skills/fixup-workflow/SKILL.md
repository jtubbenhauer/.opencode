# Fixup Workflow

## What Are Fixups

The user leaves review notes in `.opencode/fixups.md` while reading code in Neovim. Each note has a file path, line number, and description of what to fix.

## File Format

```markdown
# Review Notes

- `path/to/file.ts:42` - description of what to fix
- `path/to/file.ts:15-23` - description for a line range
```

## Processing Rules

1. Read `.opencode/fixups.md` immediately — don't search the codebase for TODOs
2. Process entries ONE AT A TIME
3. For each entry: read the referenced file, apply the fix, remove that line from fixups.md
4. After all entries are done, delete `.opencode/fixups.md` entirely

## Critical Constraints

- Only fix what the notes describe — don't revert other changes the user made during review
- If a note is unclear, ask rather than guess
- One-at-a-time processing ensures interrupted sessions don't lose remaining items

## Trigger Phrases

- "fix the notes" / "fix the fixups" / "fix the review notes"
- "address the notes I left"
- "fix my review comments"
