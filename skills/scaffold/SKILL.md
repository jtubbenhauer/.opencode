---
name: scaffold
description: Produce structural code skeletons with TODO stubs for human implementation. Triggered by "scaffold this", "stub this out", "scaffold the plan", "create stubs". Also handles "review fills" / "check my stubs" for post-implementation review.
---

# Scaffold Skill

## What This Is

A two-phase workflow for human-in-the-loop code generation:

1. **Scaffold** — the model writes structure (types, interfaces, signatures, imports, configs) and stubs method bodies with descriptive TODOs. The human fills in the bodies.
2. **Review Fills** — after the human implements, the model validates that implementations match TODO objectives and checks for errors.

The key insight: LLMs are good at architecture and structure but bad at idiomatic method bodies. This skill inverts the usual delegation — the model produces the skeleton, the human writes the code that matters.

## Phase 1: Scaffold

### Trigger Phrases

- "scaffold this"
- "stub this out"
- "scaffold the plan"
- "create stubs"

### Pre-Scaffold Checklist

Before writing any code:

1. Check `.opencode/plans/` for a matching plan file. If one exists, use it as the primary source of truth.
2. Load relevant skills for the file types involved (typescript-coding-standards, angular-coding-standards, etc.)
3. Explore the codebase for existing patterns the stubs must follow.
4. Identify which files are structural (write in full) vs which contain logic (stub with TODOs).

### What to Write in Full (No TODOs)

- Type definitions, interfaces, enums
- Constants and config objects
- Import statements and barrel exports
- Method/function signatures (name, parameters, return type)
- Any file under ~30 lines that is purely structural
- Environment config files with `// TODO: configure for staging/prod` comments only for values that need changing

### What to Stub (TODO Bodies)

Every function/method body that contains business logic gets a TODO stub.

**TODO format:**

```typescript
async validateMigration(migrationId: string): Promise<ValidationResult> {
  // TODO: Read manifest from GCS pending/ folder. Parse JSON, verify required fields
  // (migrationId, version, workspaceId, syncTrigger). Return valid/invalid with specific
  // error messages per missing field. Use getError(error) for error handling.
}
```

**Good TODO — describes objective + constraints:**

```typescript
// TODO: Query PostgreSQL for translation maps filtered by tenantId. Map rows to
// ITranslationMapGcs format. If no maps found, return empty array (not an error).
// Use existing getDatabaseConnection() from db-utils.
```

**Bad TODO — too vague:**

```typescript
// TODO: validate manifest
```

**Bad TODO — too prescriptive (writing the code in a comment):**

```typescript
// TODO: const manifest = await gcs.readFile(pendingPath);
// const parsed = JSON.parse(manifest);
// if (!parsed.migrationId) return { valid: false, error: 'Missing migrationId' };
// ... (10 more lines of pseudo-implementation)
```

**Rules for TODO stubs:**

- State the **objective** — what this function accomplishes
- State **key constraints** — error handling patterns, edge cases, return shapes
- Reference **existing utilities** by name — "Use getError() for error handling", "Use the serialise() function from utils"
- State **what not to do** when relevant — "Do NOT define a new serialised type, use the existing ITranslationMapGcs"
- Keep it to 2-4 lines per TODO
- If a method has no logic (just delegates), write it in full instead of stubbing

### File Markers

Every scaffolded file gets this header as the first line:

```typescript
// SCAFFOLDED - fill TODO stubs before removing this comment
```

This marker serves two purposes:
- The human knows which files still need work
- The review phase can find scaffolded files by searching for this string

### Anti-Patterns (Hard Rules)

These are non-negotiable. Do not do these things:

1. **No TypeGuard bloat.** Do not add `isX()` runtime type guard functions unless the plan explicitly calls for them. Most of the time, TypeScript discriminated unions and type narrowing are sufficient.
2. **No ad-hoc serialised/DTO types.** Do not define `ISomethingSerialised` or `ISomethingDto` interfaces unless the plan explicitly specifies them. Use canonical types and let transform functions handle shape changes.
3. **No type assertions.** Do not use `as X` or `as unknown as Y`. Use `unknown` with type narrowing, or use the project's `getError()` pattern for catch blocks.
4. **No cargo-culting patterns.** If an existing file has 100 lines of TypeGuard validators, that does NOT mean the new file should too. Match the pattern's intent, not its volume.
5. **No unsafety features.** Do not add retry logic, dry-run flags, confirmation prompts, or caching unless the plan asks for them.
6. **No guess-and-check environment config.** If you don't know the staging/prod values for an env var, leave a `// TODO: configure for staging/prod` comment. Do not copy dev values to all environments.
7. **No copy-paste without adaptation.** If you reference an existing file as a template, state exactly how the new file differs. Do not copy 200 lines and change 3 — write the 3-line version and import the rest.

### Scaffold Completion

After generating all files, report:

```
Scaffolded: 5 files created, 12 methods stubbed, 3 files written in full

Files written in full:
- src/types/migration-types.ts (interfaces + enums)
- src/config/migration-env.ts (env config)
- src/index.ts (barrel exports)

Files with TODO stubs:
- src/services/migration-validator.ts (6 stubs)
- src/services/gcs-reader.ts (4 stubs)
- src/handlers/validate-migration.ts (2 stubs)

Fill in the TODO stubs and run "review fills" when ready.
```

Then `mem_save` the scaffold session with topic key `scaffold/<feature-name>` so the review phase can recover context after compaction.

### Smart Model Only

Scaffolding is structural work that requires good judgment about types, interfaces, and architecture. Use the smart model (GLM-5.1, Opus, or equivalent) for scaffolding. Do not delegate scaffolding to cheap models — they make poor type decisions and the whole point is that the skeleton is correct.

## Phase 2: Review Fills

### Trigger Phrases

- "review fills"
- "check my stubs"
- "review the scaffold"

### Review Process

1. Find all files with `// SCAFFOLDED` comment headers.
2. Load the original scaffold session from memory (search engram for `scaffold/<feature-name>`).
3. For each file:
   - Check if all TODOs have been implemented (no leftover `// TODO:` comments in method bodies).
   - For each filled-in method, verify:
     - Implementation matches the TODO's stated objective.
     - Follows the project's coding patterns (imports, naming, error handling).
     - No anti-pattern violations (see list above).
     - No type errors or obvious bugs.
   - Run lint/typecheck on the file if possible.
4. Report findings per file:
   - `src/services/migration-validator.ts` — 5/6 stubs filled. Missing: `validateManifestFields()`.
   - `src/services/gcs-reader.ts` — All stubs filled. Warning: `readPendingManifest()` catches `unknown` but casts to `Error` — use `getError()` instead.
5. Remove `// SCAFFOLDED` headers from files where all stubs are filled and no issues found.
6. If issues found, list them and tell the user what to fix.

### Review Scope

- Check scaffolded files for implementation completeness and correctness.
- Run lint and typecheck. Do not run tests unless asked (tests may not be written yet).
- Do not modify code during review — report findings and let the human decide.

## Interactions with Other Skills

- **plan-writing**: If a plan file exists in `.opencode/plans/`, the scaffold skill uses it as the primary source. The plan should include architecture constraints, lint contract, and anti-patterns — the scaffold skill enforces these.
- **fixup-workflow**: If the review phase finds issues, those can be captured in `.opencode/fixups.md` for the human to address later.
- **Code quality skills** (typescript-coding-standards, etc.): Always load relevant skills before scaffolding so the structure matches project conventions.