---
description: "Prevents AI from making git commits - user handles all git operations"
globs:
  - "**/*"
---

# No Git Commits Policy

## Rule

**NEVER** create git commits. This includes:

- `git commit`
- `git commit -m "..."`
- `git commit --amend`
- Any variation of commit commands

## What To Do Instead

1. **Make your changes** - Edit files, create files, delete files as needed
2. **Stop there** - Do not stage or commit
3. **Report completion** - Tell the user what was changed

The user will handle all git operations including:

- Staging (`git add`)
- Committing (`git commit`)
- Pushing (`git push`)
- Branch management

## Why

The user prefers to review changes and craft their own commit messages. This gives them full control over git history and commit granularity.
