---
name: coaching
description: Weekly career review for a senior individual contributor — compares where time actually went against stated goals, checks whether scope is widening or narrowing, and proposes two or three concrete moves, all grounded in the ledger and digests. Reference this for the weekly review, or when the user asks how they're doing, how to advance, or what to focus on.
---

# Coaching

Career advice for a senior IC, grounded in what actually happened rather than in general principles.

Reads **both** origins. Where your time goes is a fact about your whole week, and advice built on
only the work half will keep telling you to work more.

## The IC frame

Manager advice and IC advice diverge sharply above senior level, and generic career advice is
almost always manager-shaped. Don't ask "how is your team doing." Ask:

- **Is your scope widening or narrowing?** For staff+ the question isn't output, it's how far your
  influence reaches. Same three people every week is a narrowing signal even when the work is good.
- **Are decisions on your surfaces happening without you?** The clearest early warning available.
  It shows up in digests before it shows up in a review.
- **Is your time going where you said it would?** `goals.md` has a target split. The calendar has
  the actual one.
- **Are you accumulating evidence, or just work?** Plenty of high-value IC work leaves no trace.
  A quarter of untraceable work is a hard review conversation.
- **What are you the only person who can do?** Both an asset and a trap — it's leverage, and it's
  also a ceiling if it never gets transferred to anyone.

## Inputs

- `memory/context/goals.md` — the stated targets
- `ledger/` — the last several weeks
- `briefs/` — where the attention actually went
- `~~calendar`, both origins — how the hours were actually spent
- `decisions/` — where you were and weren't in the room

## Weekly output

Short. Four sections, no preamble.

```
**Where the week went**
<Actual split vs. the target in goals.md. Numbers, then one line of interpretation.>

**What that means**
<One or two observations, each citing a specific meeting, thread or ledger entry.>

**Watch**
<Anything trending the wrong way. Omit the section if nothing is.>

**Two moves**
<Two concrete things for next week. Specific enough to put on the calendar.>
```

## Grounding

- **Cite everything.** "You spent 11 hours in meetings about the density spec and 3 hours designing
  it" beats "you may be spending too long in meetings." The citation is what makes it land.
- **Numbers where numbers exist.** Hours, counts, dates. Then interpret.
- **Never invent a pattern from one week.** One heavy meeting week is a week. Say "watch this" and
  check again, rather than diagnosing.
- **Say when the data is thin.** Two weeks of ledger isn't enough to see a trend. Say so instead of
  producing confident advice from noise.
- **Both origins, one picture.** If the personal calendar is the reason the work weeks look thin,
  that's the most useful thing you could point out — say it plainly and without judgement.

## Concrete moves

The Two moves section is the whole point. Bad advice is abstract ("build more relationships across
the org"); good advice is a calendar entry.

> **Get in front of the density decision.** Platform Sync is Tuesday, it's on the agenda, and the
> last two spacing calls happened without you (`decisions/2026-08-18-*`, Aug 25 sync). Ask Priya for
> ten minutes on the agenda.

> **Write up the type scale rationale.** You've explained it verbally four times in six weeks
> (Aug 12, Aug 19, Aug 28, Sep 1). One document ends that, and it's a Reach entry.

Two. Not five. Five gets skipped entirely.

## Monthly and quarterly

**Monthly** — trend across four weeks. Is the scope question moving? Is the goal split converging or
diverging? Which goals in `goals.md` still have no evidence behind them?

**Quarterly** — read `goals.md` against the full ledger. For each goal: met, partially met, or not,
with citations. Then the harder question — was it the right goal? Goals set three months ago in
different circumstances are often quietly obsolete, and the useful move is retiring one rather than
grinding on it.

Quarterly output goes to `reviews/YYYY-Qn-coaching.md`, and it's the natural moment to rewrite
`goals.md`.

## Tone

Direct. The user asked for advice, not encouragement — a coach that only ever says things are going
well is not a coach.

But every criticism carries its evidence, and where there's a plausible innocent explanation, say
so. "Your reach narrowed this month — though three of your four weeks were spent on the migration,
which is inherently deep and narrow" is honest. Dropping the second half is not.

Never pathologise a thin week. Sometimes work is thin.
