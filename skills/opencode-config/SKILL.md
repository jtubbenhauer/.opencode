# OpenCode Config

Meta-skill for editing the opencode setup itself.

## Directory Layout

```
~/.config/opencode/
├── opencode.json          # Main config: MCPs, watcher, instructions paths, agent overrides
├── tui.json               # TUI appearance settings
├── AGENTS.md              # Global agent instructions (loaded for all agents)
├── agents/                # Custom agent definitions (.md files)
│   ├── implement.md       # Surgical code implementer
│   └── plan.md            # Planning specialist (shadows built-in)
├── instructions/          # Global instructions (.instructions.md)
│   ├── fixup-notes.instructions.md
│   ├── git-discipline.instructions.md
│   └── principle-build-restriction.instructions.md
├── plugins/               # TypeScript plugins
│   └── git-ai.ts
└── skills/                # Personal skills (SKILL.md in each subfolder)
```

## Agent Conventions

- Agents are `.md` files in `agents/` with YAML frontmatter
- Required frontmatter: `description`, `mode` (primary/background), `model`, `temperature`
- Optional: `color`, `permission` (bash/read/edit/write allowlists)
- Naming an agent the same as a built-in (e.g., `plan.md`) shadows the built-in
- Disable built-ins via `opencode.json`: `"agent": { "name": { "disable": true } }`

## Secrets

- Use `{env:VAR_NAME}` interpolation in `opencode.json`
- Actual values live in `~/.zshrc` as exports
- Never commit secrets — `.env*` is gitignored

## MCP Servers

- Defined in `opencode.json` under `"mcp"`
- Types: `"local"` (runs command) or `"remote"` (URL + headers)
- Environment vars passed via `"environment"` block with `{env:...}` references

## Instructions

- Files in `instructions/` with `.instructions.md` extension
- YAML frontmatter: `description`, optional `globs` for scoping
- Referenced in `opencode.json` `"instructions"` array

## Skills

- Each skill is a directory with a `SKILL.md` file
- Can live in: project `.opencode/skills/`, `~/.config/opencode/skills/`, `~/.agents/skills/`
- Loaded on-demand via the `skill` tool, not automatically
