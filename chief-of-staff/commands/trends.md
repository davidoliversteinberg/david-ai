---
description: Weekly craft run — design-system drift, then UX/UI trends worth stealing
---

# Trends

The craft half of the week. Two scans, inward then outward.

`/chief-of-staff:trends [--system-only | --trends-only]`

Both halves live here because they answer the same question from opposite directions: *is our stuff
getting worse, and is anyone else's getting better.*

> Placeholders like `~~design` are roles, not products — see [CONNECTORS.md](../CONNECTORS.md).

## Half one — system drift

Load [system-watchtower](../skills/system-watchtower/SKILL.md).

Scans `~~code` diffs and `~~design` file activity against
`memory/context/system-rules.md`. Only rules with a **Detect** line are checkable.

**First run establishes a baseline and reports nothing.** Existing violations get counted into
`reviews/watchtower-baseline.md`; from then on you report new drift only.

If a rule names a repo or file you can't reach, say so and skip it. Never report all-clear on
something you couldn't read.

Watch for clusters. Three instances of the same violation isn't three typos, it's a broken process —
usually a generator with a stale prompt. Say which one you're looking at.

## Half two — outward

Load [trend-radar](../skills/trend-radar/SKILL.md).

Reads the pinned list in `memory/context/trend-sources.md`, not the open web. If that file is empty,
skip this half and say so — an unpinned search returns SEO content every time.

Two bars, both required: names a specific technique, **and** maps to a named surface you own.
Items clearing one are dropped, not padded.

Zero items is an honest week. Report it in one line plus the closest near-miss and which bar it
failed.

## Output

```
**Drift** — new violations since last run, or "nothing new"
**Worth stealing** — items clearing both bars, or "nothing this week"
```

Append trend items to `reviews/trends.md`. Third appearance in five weeks from independent sources
is a real signal — say so explicitly when it happens. Recurrence tracking is most of the value.

Write the run to `reviews/craft-YYYY-MM-DD.md`.

## Never

Don't fix code unprompted. Report; offer interactively; scheduled runs touch nothing.
