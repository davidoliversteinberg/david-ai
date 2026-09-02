---
description: Weekly review — impact recorded, then honest coaching on where you're going
---

# Review

End of week. Two halves: what happened, then what it means.

`/chief-of-staff:review [--monthly | --quarterly]`

> Placeholders are roles, not products — see [CONNECTORS.md](../CONNECTORS.md).

## Run

**Half one — the record.** Run [ledger](ledger.md). Work origin only. Get the entry confirmed and
written before doing any interpretation; the coaching reads it.

**Half two — the interpretation.** Load [coaching](../skills/coaching/SKILL.md). **Both origins** —
where the time went is a fact about the whole week, and advice built on the work half alone will
always conclude "work more."

Output inline; write to `reviews/YYYY-Www.md`.

## Weekly output

```
**The week**       — ledger entry, four categories
**Where time went** — actual split vs. goals.md, with numbers
**What that means** — one or two observations, each citing something specific
**Watch**          — anything trending wrong; omit if nothing is
**Two moves**      — two concrete things for next week
```

Two moves. Not five.

## Monthly

`--monthly` reads four weeks. Trend questions: is scope widening or narrowing, is the goal split
converging, which goals still have no evidence behind them.

Also run the memory hygiene pass — `CLAUDE.md` over ~100 lines, stale ignore rules, people files
with old `last-interaction` dates.

## Quarterly

`--quarterly` reads `goals.md` against the whole quarter's ledger. Per goal: met, partly, or not,
with citations.

Then the harder question, which is the one worth the time: **was it the right goal?** Goals set
three months ago under different conditions are often quietly obsolete, and retiring one is usually
a better move than grinding on it.

End by rewriting `goals.md` for the next quarter. Keep the old version — the drift between them is
itself a signal worth reading next time.

Output to `reviews/YYYY-Qn-coaching.md`.

## Tone

Direct, and always with evidence. A coach that only says things are going well isn't one.

But where there's a plausible innocent explanation for something that looks bad, say it. And never
pathologise a thin week — sometimes work is thin.
