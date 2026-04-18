---
description: "Restrictions on building the principle monorepo"
globs:
  - "**/principle-theorem/**"
  - "**/dentaleye/**"
---

# Build Restrictions for Principle Monorepo

## NEVER Run Build Commands

**BLANKET BAN**: Do not run ANY build commands in the principle-theorem repository (or dentaleye branch). This includes:

- `pnpm nx run <any-project>:build`
- `pnpm nx build <any-project>`
- `pnpm nx affected:build`
- `nx run-many --target=build`
- Any variation of build commands

## What To Do Instead

1. **For verification**: Use `lsp_diagnostics` on changed files
2. **For type checking**: Use `lsp_diagnostics` - it catches type errors without building
3. **For testing**: `pnpm nx run <project>:test` is allowed (tests don't require full builds)
4. **For linting**: `pnpm nx run <project>:lint` is allowed
5. **Ask the user**: If you absolutely need to verify a build, ask the user to run it manually

## Why

Builds in this monorepo consume excessive memory (16GB+) and will crash the system, especially if dev servers or other processes are running. The LSP provides sufficient type checking for verification purposes.
