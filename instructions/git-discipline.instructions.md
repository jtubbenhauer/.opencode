---
description: "Git discipline -- default to not committing, but allow it when context requires"
globs:
  - "**/*"
---

# Git Discipline

## Default Behavior

Don't create git commits unless:

- The user explicitly asks you to commit
- The workflow requires it (e.g., PR creation, review work)
- You're working in a repo where the user has established commit expectations

## When Committing

- Let the user craft commit messages when possible
- If asked to commit, write a concise message that describes the "why" not the "what"
- Never force push to main/master without explicit confirmation
- Never amend commits that have been pushed

## When in Doubt

Ask. "Should I commit this, or do you want to handle git yourself?"
