---
name: shared-gather
description: Fetches calendar, mail and chat once and caches the result so every other skill reads the same snapshot instead of hitting connectors again. Reference this before any skill gathers from a source, when several skills run close together, or when a run hits a rate limit.
---

# Shared Gather

One fetch, many readers.

Without this, every scheduled task does its own gather. Six tasks in a week means six full sweeps of
the same inbox, six chances to hit a rate limit, and six slightly different views of the same
Tuesday. The Monday radar runs nine minutes after the Monday brief — there is no version of "the
last two weeks of chat" that meaningfully changed in nine minutes.

## The cache

`<workspace>/cache/YYYY-MM-DD/<kind>.md`, one file per source kind: `calendar`, `mail`, `chat`,
`tracker`.

Every file opens with a header that makes it safe to reuse:

```markdown
---
kind: chat
gathered: 2026-09-04T08:59:12-04:00
window: 2026-08-21 .. 2026-09-04
sources: ms365-teams
complete: false
truncation: "searched 46 of 48 chats before a Microsoft Graph 429"
---
```

`complete: false` is the important field. A reader that treats a truncated sweep as a full one will
report silence it never verified.

## Before you gather

1. Look for `cache/<today>/<kind>.md`.
2. If it exists, is younger than the freshness window, and its `window` covers what you need — **use
   it. Do not call the connector.**
3. Otherwise gather, write the cache file, then use it.

| Kind | Fresh for | Why |
|---|---|---|
| `calendar` | 4 hours | Meetings move, but rarely inside a morning |
| `mail` | 2 hours | |
| `chat` | 2 hours | |
| `tracker` | 12 hours | Board state is slow |

A task that needs *today's* activity after the working day — the evening capture — will always find
the morning cache stale and gather fresh. That is correct, not a cache miss to engineer around.

## Say where it came from

Any artifact built on cached data says so, once, at the end:

> *Chat and mail from the 08:59 cache; calendar fetched fresh.*

This is not bookkeeping. When something is missing from a brief, the first question is whether it
was filtered out or never fetched, and the cache header answers it in one line.

**Never present cached data as live.** If the cache says `complete: false`, every artifact reading it
inherits that caveat and repeats it.

## Writing the cache

Store what was actually returned, lightly normalized — sender, timestamp, thread, verbatim body or a
faithful excerpt. Do not summarize into the cache. A summary is a judgement, and the next skill to
read this file may be making a different judgement than the one that wrote it.

Timestamps go in the user's local timezone, converted once, here. Sources that report UTC regardless
of the real zone are converted at cache-write time so that no downstream skill has to remember.

## Hygiene

The cache is disposable. It holds raw message content and belongs in `.gitignore` even in a private
workspace — there is no version-control value in yesterday's inbox, and it is the highest-volume
sensitive content the tool touches.

Prune directories older than 7 days on the monthly hygiene run.

## When not to cache

- **Interactive runs where the user is waiting on current state.** If they ask "did Raff reply yet",
  fetch. The cache exists to stop scheduled tasks stampeding, not to make a live question stale.
- **Anything read once and never re-read** — a specific meeting transcript fetched for one digest.
