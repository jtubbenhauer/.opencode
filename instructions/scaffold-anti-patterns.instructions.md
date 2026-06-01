---
description: "Anti-patterns to avoid when generating code — applies to scaffolding, implementation, and any code generation. Always loaded."
globs:
  - "**/*.ts"
  - "**/*.tsx"
  - "**/*.js"
  - "**/*.jsx"
  - "**/*.go"
---

# Code Generation Anti-Patterns

Hard rules for any model generating code. These are non-negotiable, not suggestions.

## Type Safety

- **No TypeGuard bloat.** Do not add `isX()` runtime type guard functions unless explicitly requested. Use TypeScript discriminated unions and type narrowing instead.
- **No ad-hoc serialised/DTO types.** Do not define `ISomethingSerialised` or `ISomethingDto` variants unless the plan explicitly specifies them. Use canonical types and let transform functions handle shape changes.
- **No type assertions.** No `as X`, no `as unknown as Y`. Use `unknown` with type narrowing. Use `getError(error)` in catch blocks instead of casting to `Error`.

## Environment & Config

- **No guess-and-check env values.** If you don't know the staging/prod value, leave a `// TODO: configure for staging/prod` comment. Do not copy dev values across all environments.

## Pattern Discipline

- **No cargo-culting.** An existing file having 100 lines of TypeGuard validators does not justify adding them to a new file. Match the pattern's intent, not its volume.
- **No copy-paste without adaptation.** If referencing an existing file as a template, state exactly how the new file differs. Do not copy 200 lines and change 3 — import shared code and write only the differences.
- **No unsafety features.** Do not add retry logic, dry-run flags, confirmation prompts, logging wrappers, or caching unless the plan asks for them. Implement what was requested, nothing more.

## Lint Contract

When writing code for projects with strict lint configs:
- Must pass `eslint --max-warnings 0`
- No `@typescript-eslint/no-unsafe-*` violations
- No `@typescript-eslint/no-explicit-any` violations
- No prettier formatting errors
- If you don't know the project's lint rules, check the eslint config before writing code