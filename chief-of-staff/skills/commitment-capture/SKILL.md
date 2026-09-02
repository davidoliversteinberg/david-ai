---
name: commitment-capture
description: Extracts commitments from meetings, chat and email into TASKS.md with provenance and a status token, and keeps the task board in sync. Reference this when the user asks to capture todos, after a digest run, when reviewing proposed tasks, or when a task's status changes.
---

# Commitment Capture

Turns "I'll get you the tokens by Thursday" into a task that still knows where it came from.

Load [source-adapter](../source-adapter/SKILL.md) first.

## What counts as a commitment

**Capture:**
- You said you'd do something. Explicit or implied — "I'll take that" and "sure, I can look" both count.
- Someone assigned you something and you didn't decline.
- Someone committed something *to* you with a date → `@waiting`.
- A deadline was stated that involves you.

**Don't capture:**
- Hypotheticals. "We could do X" is not a commitment.
- Things already tracked in `~~tracker` — link to the ticket instead of duplicating it.
- Anything already in `TASKS.md`. Check by meaning, not by string match; the same commitment gets
  worded differently in a meeting and in the follow-up email.
- Standing meetings and recurring obligations. Those are calendar, not tasks.

Ambiguous cases go into `## Proposed` rather than being dropped. Under-capturing is worse than
over-proposing, because a missed commitment is invisible.

## Format

Sections stay as they are, for board compatibility. The status token is the addition.

```markdown
- [ ] **Reconcile design-playground token scale with Dan's agent-built UI** `@todo`
  - source: Meeting · Virtual Design Teammates Sync #2 · 2026-08-20 · work
  - added: 2026-08-21
  - due: before Opticon
```

- **Title in bold**, imperative, specific enough to act on cold. "Follow up with Dan" is a bad
  title; "Reconcile the token scale with Dan's agent-built UI" is a good one.
- **`source:`** — `<type> · <where> · <date> · <origin>`, and a link when there is one. This is what
  the board shows, and it's the difference between a task list and a pile of strings.
- **`added:`** — when the plugin captured it, not when it was said.
- **`due:`** — only when a real date or event was stated. Never invent one.

## Status tokens

| Token | Means | Section |
|-------|-------|---------|
| `@todo` | yours, actionable now | Active |
| `@waiting` | blocked on a named person | Waiting On |
| `@blocked` | blocked on a thing, not a person | Active |
| `@done` | finished | Done |
| `@ignored` | consciously dropped, kept for the record | Done |

`@waiting` must name who: `@waiting` + a `waiting-on:` sub-bullet. An unattributed `@waiting` is
just a task you're avoiding.

`@ignored` exists because Done and Deleted aren't the same thing. Dropping something on purpose is a
decision, and six weeks later you'll want to know you made it.

## Interactive vs scheduled

**Interactive** — propose the batch, get a yes, then write:

```
Found 4 commitments:
  1. Reconcile the token scale with Dan — Design Sync, Aug 20 — work
  2. Send the density spec to Priya by Thu — email, Aug 21 — work
  3. Reply to the conference CFP — personal email, Aug 19 — personal
  4. [unclear] "we should look at the empty states sometime" — Design Sync — capture as Someday?

Add 1–3, and 4 to Someday?
```

**Scheduled** — never write to Active unattended. Append to a `## Proposed` section at the top of
`TASKS.md`:

```markdown
## Proposed
<!-- from cos-evening-capture, 2026-09-02 -->

- [ ] **<title>** `@todo`
  - source: ...
```

The next interactive session triages it. Say how many are pending when the user next opens their
tasks. If `## Proposed` grows past about fifteen items, that's a signal the capture is too loose —
say so rather than continuing to pile up.

## Dedupe

Before adding anything, read the whole of `TASKS.md` including Done. Match on meaning:

- Same commitment from a meeting and its follow-up email → one task, both sources listed.
- A task already in Done that resurfaces → don't re-add; flag it as "this came up again."
- Near-duplicates with different due dates → one task, the earlier date, both sources.

## Status changes

When something in a digest shows a task moved:

- Someone delivered what you were `@waiting` on → flip to `@todo`, note who and when.
- You said in a thread that something is done → propose `@done`, don't assume.
- A `@waiting` item is past its date with no movement → surface it in the next brief under
  "commitments to you that went quiet."

## The board

`board.html` in the workspace reads and writes the same `TASKS.md`. It watches for external changes,
so a file edit and a board edit converge. If both change simultaneously the file wins — tell the
user when that happens rather than silently discarding board state.

Copy it from `${CLAUDE_PLUGIN_ROOT}/skills/assets/board.html` on first run if it isn't there.

Don't run `open` on it. In Cowork the agent is in a VM and the command won't reach the user's
browser. Say: "Board's at `board.html` — open it from your file browser."
