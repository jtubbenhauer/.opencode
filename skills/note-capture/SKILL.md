---
name: note-capture
description: Capture notes to the personal Quartz site. Triggered by "add to notes", "note this", "save to notes", "log this", "note that down", "add a note".
---

# Note Capture

Capture notes to the personal notes site (Quartz) at `~/dev/quartz/content/`.

## Trigger Phrases

- "add to notes", "note this", "save to notes", "log this"
- "note that down", "add a note"

## Content Structure

```text
content/
├── work/
│   ├── daily/YYYY-MM/DD.md       ← timestamped daily entries
│   ├── {project}/                  ← project folders (migration-toolkit/, commbank-smarthealth/, etc.)
│   │   ├── index.md                ← project overview
│   │   └── {topic}.md              ← specific topic/plan within the project
│   └── fixes/                      ← small one-off bug fixes
│       └── {slug}.md
├── personal/
│   ├── daily/YYYY-MM/DD.md
│   └── ...
```

## Routing Decision Tree

1. **Is this personal?** → `personal/daily/YYYY-MM/DD.md`
2. **Is this about a known project?** (check existing folders under `work/`) → append to or create a file in that project folder
3. **Is this a small fix or one-off task?** → `work/fixes/{slug}.md`
4. **Is this general daily work?** → `work/daily/YYYY-MM/DD.md`
5. **Unsure?** → default to `work/daily/YYYY-MM/DD.md`

To check existing project folders, list `~/dev/quartz/content/work/` and look for directories (excluding `daily/`, `fixes/`, `board.md`, `index.md`).

## Behaviour

### Daily entries (work/daily/ or personal/daily/)

1. Create file with frontmatter if new (see template)
2. Append entry at the end
3. Use `## HH:MM` heading with **1 bullet per task** — one line, one dot point

### Project topic files (work/{project}/{topic}.md)

1. If the topic file exists, append under a new `## HH:MM` timestamp heading
2. If creating a new topic file, use the topic template
3. If the project folder doesn't exist yet, **create it** with an `index.md`

### Fixes (work/fixes/{slug}.md)

1. One file per fix, kebab-case slug
2. Use the fix template — no timestamp headings, just content

## Templates

### Daily file

```markdown
---
title: "YYYY-MM-DD"
tags: [work]
---

## HH:MM

- Did X
```

Tags: `[work]` or `[personal]`. Add topic tags as relevant.

### Project index (new project folder)

```markdown
---
title: "Project Name"
tags: [work, project-tag]
---

Brief description of what this project is.
```

### Topic file (within a project)

```markdown
---
title: "Topic Name"
tags: [work, project-tag]
---

## HH:MM

- bullet point
```

### Fix file

```markdown
---
title: "Fix description"
tags: [work, fix]
---

- What was broken
- What caused it
- How it was fixed
```

## Permissions

This skill MAY:
- Create new project folders under `work/` with an `index.md`
- Create new topic files within existing project folders
- Move a file from `fixes/` into a new project folder if the topic grows

This skill must NEVER:
- Push to git (the systemd timer handles that)
- Delete files
- Modify the board files (that's the board skill's job)

## Hard Constraints

- No prose paragraphs. Bullets only.
- No preamble ("Today I learned...", "Here's a note about...")
- **Daily entries: ONE bullet per task. One line. No sub-bullets. No implementation details. No file paths. No plan links.** The daily log is "what I did", not "how I did it". Implementation detail goes in project topic files, not daily files.
- Example GOOD daily entry: `- Implemented D4W MediaSuite upload toolkit integration`
- Example BAD daily entry: `- Added MediaSuite to ImagingSoftwareType enum, gated to D4W-only via PMS_IMAGING_SOFTWARE_TYPES. Updated groupImagesByPatient to support both mediasuite/ and imaging-mediaSuite/ bucket prefixes. Files: migration-toolkit.ts, generate-upload-code.component.ts`
- Code blocks: short. If it needs more than 10 lines, link to the file instead.
- **Code blocks MUST always specify a language tag** (e.g. ` ```typescript `, ` ```sql `, ` ```json `). Use ` ```text ` for log output, plain text diagrams, or anything that isn't a programming language. Never write a bare ` ``` ` opening fence.
- Timestamp each entry in daily files and project topic files
- Always use AEST (UTC+10) for timestamps. Run `date +"%H:%M"` to get the correct time — never guess.
- File paths are always absolute: `~/dev/quartz/content/...`

## Tag Conventions

- Always include context: `work` or `personal`
- Add 1-2 topic tags (e.g. `angular`, `firestore`, `migration-toolkit`)
- Keep tags lowercase, no spaces
