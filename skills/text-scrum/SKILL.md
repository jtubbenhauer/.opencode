---
name: text-scrum
description: Format async/text scrum updates (Slack, etc.) with Yesterday/Today bullets and inline so-whats. Use when prepping text-mode scrum or when the user asks to "format scrum", "text scrum", "scrum notes". For spoken/video scrum, use get-to-the-point instead.
---

# Text Scrum

Use when prepping async/text scrum updates (Slack, etc.), or when the user
asks to "format scrum", "text scrum", "scrum notes", or similar. Triggers on
files like `tmp/scrum.md`, `**/scrum*.md`, `**/standup*.md`.

For spoken/video scrum, use `get-to-the-point` instead.

## Why this exists

Text scrum is points discipline in a different shape. Same brain — "is this
a defensible claim or just a topic?" — but the deliverable is a filtered log,
not a cue card. Treat text days as practice for video days.

## Format

Two sections, `Yesterday` and `Today`. Flat bullets under each. Each bullet:

> **Headline** - brief context, status, or so-what

Examples:

- `Automatic marketing charges well beyond spend needed - testing, chased a bug in cost forecasting (missed deploying a function)`
- `Reports of users not being notified of patient arrivals - seems like a recent Windows update is affecting this`

The dash carries the "so what". No separate insight line, no scripted
phrasing — write it like you'd type it in Slack.

## Nesting

Default flat. Nest only when a parent bullet has 2+ genuinely distinct
sub-items that wouldn't make sense on their own:

- ✅ Nest: "PR reviews" with three specific PRs underneath
- ❌ Nest: one sub-bullet adding detail — fold it into the parent with a dash

## Triage every bullet

Same classification as spoken scrum:

- **Point** — defensible claim, has a so-what
- **Status** — where time went; earns its place if the time was real
- **Drop** — noise, or only valuable as evidence for a point above

Text mode is more permissive than spoken: status bullets stay if they
represent meaningful work. But apply the "I believe that..." test to kill
filler. If a line is just "worked on stuff", drop it or merge it.

## Group before you cut

Same as spoken: multiple status lines often collapse into one stronger point.
Look for the pattern before you finalise.

> Three "migration delayed / TeamViewer broken / card issue" lines
> → one bullet: "non-technical blockers killing migration prep this week -
>   three separate incidents"

## Output format

Hand back two things:

1. **Annotated notes** — the user's original bullets tagged `[POINT]`,
   `[STATUS]`, or `[DROP]`, with one-line rationale where non-obvious.

2. **Final scrum** — the bullets in delivery shape, ready to paste:

   ```
   Yesterday
   - <bullet>
   - <bullet>

   Today
   - <bullet>
   - <bullet>
   ```

   Asks/handoffs woven inline (e.g. "chat with @shoda re feedback form"),
   not as a separate section.

## Anti-patterns

- Don't expand short notes into long bullets
- Don't promote status to point with weasel words ("interesting...", "notable...")
- Don't strip context the reader needs — text scrum is read cold, not spoken
- Don't bury the headline behind narrative
