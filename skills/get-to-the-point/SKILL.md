# Get to the Point

Use when prepping scrum/standup notes, or when the user asks to "find my points",
"get to the point", or similar. Triggers on files like `tmp/chat.md`,
`tmp/scrum.md`, `**/scrum*.md`, `**/standup*.md`.

## What a "point" is

From Joel Schwartzberg, *Get to the Point*: a point is a defensible claim, not
a topic. Test it by prefixing "I believe that..." — if the sentence still
makes sense, it's a point.

- ✅ Point: "I believe that our migration prep is being wasted by non-technical
  blockers — three this week."
- ❌ Topic: "Migration toolkit v2 branch."

## Triage every bullet

Classify each line in the user's notes as one of:

- **Point** — defensible, has a "so what", action or insight follows
- **Status** — where time went; necessary context but flat on its own
- **Drop** — noise, or only valuable as evidence for a point above

Aim for **1–3 points** per update. If everything looks like a point, nothing is.

## Group before you cut

Multiple status bullets often collapse into one point. Look for the pattern
across items before discarding any.

> Three separate "migration delayed / TeamViewer broken / card issue" bullets
> → one point: "non-technical blockers are killing migration prep this week."

## Output format

Hand back two things by default:

1. **Annotated notes** — the user's original bullets, each tagged
   `[POINT]`, `[STATUS]`, or `[DROP]`, with one-sentence rationale where
   non-obvious.

2. **Cue card** — the points only, in delivery order, in the user's own
   shorthand. No full sentences, no scripted phrasing. The user speaks
   naturally around each anchor.

   Format:

   ```
   1. <point in 5–10 words, user's voice>
      → so what: <one-line reason it matters>

   2. ...
   ```

   Plus a one-line **status** roll-up and any **asks** as separate bullets.

Only produce a full spoken script if the user explicitly asks for one.

## Delivery

The cue card is an anchor, not a script. Talk naturally around each point;
elaborate where it helps, stop when the point is made. The "I believe
that..." discipline applies to the anchor, not to every sentence said out
loud.

Embellishing ≠ waffling:

- ✅ Elaborating to support a point
- ❌ Filler words while thinking of the next point

## Anti-patterns

- Don't force a point per bullet — silence is better than fake insight
- Don't promote status to point to fill space
- Don't expand short notes into long updates
- Don't bury the point inside narrative or caveats
