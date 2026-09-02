---
description: Catch up on a window — meetings, chat and email summarized and ranked
---

# Digest

Backward-looking. What happened over a window, and what you have to do about it.

`/chief-of-staff:digest [--since 7d | --since 2026-08-25 | today | yesterday]`

Default window: since the last digest, or 24 hours if there isn't one.

Use [brief](brief.md) for the forward-looking daily version.

> Placeholders like `~~chat` are roles, not products — see [CONNECTORS.md](../CONNECTORS.md).

## Run

1. Load [source-adapter](../skills/source-adapter/SKILL.md).
2. Load [signal-filter](../skills/signal-filter/SKILL.md).
3. Load [meeting-digest](../skills/meeting-digest/SKILL.md) and follow it — it owns the format,
   the ranking and the grounding rules.
4. Gather everything before ranking anything.
5. Output inline and write to `briefs/digest-YYYY-MM-DD.md`.

## After

Offer, in this order — one at a time, not as a menu:

1. **Commitments found** → [commitment-capture](../skills/commitment-capture/SKILL.md).
2. **Decisions detected** → [decision-log](../skills/decision-log/SKILL.md).
3. **Memory worth updating** → [work-memory](../skills/work-memory/SKILL.md).

In a scheduled run, do (1) into `## Proposed` and skip the rest.

## Long windows

Past about five days, meetings and threads stop being individually interesting and the useful output
is themes. Above roughly ten meetings, group the Meetings section by topic rather than listing
chronologically, and say you did.

Coming back from leave is the main case here. What matters is "here are the four things that changed
while you were out," not a day-by-day replay.

## Transcripts

If transcripts aren't reachable, say so once at the top of the Meetings section and degrade per
`meeting-digest`. Never write a summary of a meeting you have no content for.
