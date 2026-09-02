---
name: decision-log
description: Records design and technical decisions as short dated files capturing what was decided, what was rejected and why, and who was in the room. Reference this when a digest detects a decision, when the user says something was decided, or when they ask why something is the way it is.
---

# Decision Log

Senior ICs re-litigate the same decision for years. Someone new joins, asks why the type scale
starts at 12, and nobody can remember — so it gets reopened, and the same three hours happen again.

The rationale is the valuable part, and it's the part that never gets written down. The decision
itself usually survives in the code.

## When to write one

A decision qualifies if **reversing it later would cost real work**, and someone will eventually ask
why.

Write one for:
- A choice between viable alternatives where the losing option was defensible.
- Anything that constrains future work — a pattern, a boundary, a convention.
- A deliberate exception to an existing rule.
- A decision to *not* do something. These are the ones most likely to be forgotten and re-proposed.

Skip:
- Decisions with only one real option.
- Anything already documented in an ADR or RFC — link to it from the project file instead.
- Choices you can reverse in an afternoon.

## Format

`decisions/YYYY-MM-DD-<slug>.md`:

```markdown
---
date: 2026-09-01
status: decided
surfaces: [density spec, type scale]
people: [David Steinberg, Priya Raghavan, Dan Osei]
origin: work
---

# Density spec ships behind a flag

## Decided

The density spec ships behind a feature flag through Q4 and becomes default in the January release.

## Why

Three teams are mid-quarter on layouts built against the current spacing. A hard cutover would
break them during their release window.

## Rejected

- **Hard cutover in October.** Cleaner, no flag debt, and it's what the platform team wanted. Lost
  because the timing was the problem, not the change — this is worth revisiting for the next
  breaking change if it lands early in a quarter.
- **Ship as a separate component set.** Rejected: two parallel sets is the drift we're trying to
  avoid in the first place.

## Where

Platform Sync, Sep 1 — [thread](<url>)

## Revisit when

January release ships, or if a fourth team hits the same conflict before then.
```

**Rejected** is the section that earns the file. A decision without its alternatives is just a
statement; with them it's an argument someone can evaluate later against changed circumstances.

**Revisit when** is what stops the log becoming a graveyard. A decision made under conditions that
have since changed should be reopened, and this line is how you notice.

## Detecting decisions

`meeting-digest` flags candidates. Signals:

- Explicit — "we're going with X", "decided", "let's lock that in".
- Implicit — an argument that stops, followed by everyone acting as if X is true.
- Escalation resolved — someone above the discussion made a call.

Then check: were alternatives actually considered? If yes, it's a decision worth logging. If the
"alternatives" were never live, it's just work.

**Don't infer a decision from silence.** A thread that trails off is an open question. Log it as
open, or not at all.

## Writing

Always propose, never write unattended. The rationale usually isn't in the transcript — it's in the
user's head, and the thing worth capturing is often what *wasn't* said out loud.

```
Looks like the density flag question got settled in Platform Sync today.
Log it? I have the what and the where; I'd want a line on why the October
cutover lost — was it purely timing?
```

One question, the one that produces the rationale. Not a form.

If the user declines, drop it. A half-remembered decision file is worse than none.

## Status

- `decided` — settled and in effect
- `superseded` — replaced; link forward to the new file
- `revisit` — the revisit condition fired

Never delete a decision file. Supersede it. The history of how a decision changed is often more
useful than the current state.

## Feeds

- `impact-ledger` reads these for the **Influenced** category, which is why the *Where* link matters.
- `memory/projects/<slug>.md` gets a one-line pointer per decision.
- When someone asks "why is it like this," this directory is the first place to look.
