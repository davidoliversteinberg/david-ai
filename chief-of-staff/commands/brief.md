---
description: Today's brief — what needs you, what's coming, what went quiet
---

# Brief

The daily one. Forward-looking: today and the next 48 hours.

Use [digest](digest.md) instead for catching up on a window that's already passed.

> Placeholders like `~~calendar` are roles, not products — see [CONNECTORS.md](../CONNECTORS.md).

## Run

1. Load [source-adapter](../skills/source-adapter/SKILL.md) — resolve roles from `PROFILE.md`.
2. Load [signal-filter](../skills/signal-filter/SKILL.md) — the rules.
3. Gather:
   - `~~calendar` — today and tomorrow, all sources
   - `~~chat` and `~~email` — since the last brief in `briefs/` (or last 24h)
   - `TASKS.md` — anything due inside 48h, and `@waiting` items past their date
   - the previous brief — to build the Resolved section
4. Rank with [meeting-digest](../skills/meeting-digest/SKILL.md)'s IC ordering.
5. Write to `briefs/YYYY-MM-DD.md`, and output inline.

## Format

```
**Needs you** — at most 5, each with a quote and a link
**Today** — the calendar, one line each, with prep notes where they matter
**Due soon** — commitments inside 48h
**Went quiet** — @waiting items past their date
**Resolved** — off the last list, with what changed
```

Omit empty sections. "Nothing needs you today, three meetings, here they are" is a good brief.

## Meeting prep

For each meeting today, one line if there's something to say: the last decision on that topic, an
open question from last time, or a commitment of yours coming due in it. Only where it's real —
don't annotate a standup.

## Origin

With both work and personal sources, section Today and Due soon by origin so the day is scannable.
Rank **Needs you** across both — a personal deadline can outrank a work FYI, and pretending
otherwise is what makes assistants annoying.

## Scheduled runs

Write the file. Don't ask questions. If something needs a decision, put it in **Needs you** and let
the user handle it when they read it.
