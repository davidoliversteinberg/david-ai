---
description: Pull commitments out of recent activity into the task list
---

# Capture

Scans recent activity for things you said you'd do, and things people said they'd do for you.

`/chief-of-staff:capture [--since 24h] [--review]`

`--review` skips the scan and triages whatever is already sitting in `## Proposed`.

> Placeholders like `~~email` are roles, not products — see [CONNECTORS.md](../CONNECTORS.md).

## Run

1. Load [source-adapter](../skills/source-adapter/SKILL.md) and
   [commitment-capture](../skills/commitment-capture/SKILL.md).
2. Gather `~~chat`, `~~email`, and any meeting content in the window.
3. Extract candidates. Read all of `TASKS.md` first — including Done — and dedupe by meaning.
4. Present the batch with sources. Get a yes. Write.

Don't write to Active without confirmation, even interactively. The point of the source line is
trust, and a task list that grows on its own stops being trusted quickly.

## Triage mode

`--review` reads `## Proposed` and walks it:

```
7 proposed tasks from the last three evening runs.

  1. Reconcile the token scale with Dan — Design Sync, Aug 20
     [a]dd  [s]omeday  [i]gnore  [d]elete
```

Keep it fast. `@ignored` items move to Done rather than vanishing — a conscious drop is a decision
worth keeping.

If `## Proposed` is over about fifteen items, say so: the capture is too loose and the extraction
rules need tightening, not more triage.

## Status sweep

While you're here, check the existing list:

- `@waiting` items past their date → flag them
- Tasks whose source thread shows they're done → propose `@done`
- Tasks with no activity in 30 days → ask whether they're Someday or `@ignored`
