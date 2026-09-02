---
description: Record this week's impact, or roll it up into a promo case
---

# Ledger

`/chief-of-staff:ledger` — this week's entry
`/chief-of-staff:ledger --rollup [--since 2026-01-01]` — the promo packet draft

> Placeholders are roles, not products — see [CONNECTORS.md](../CONNECTORS.md).

## Run

1. Load [source-adapter](../skills/source-adapter/SKILL.md). **`work` origin only** — filter before
   gathering, not after.
2. Load [impact-ledger](../skills/impact-ledger/SKILL.md) and follow it.

## Weekly

Read the week's `briefs/`, `decisions/`, tasks moved to `@done`, plus `~~code` merges and
`~~tracker` closures if available. Draft under Shipped / Unblocked / Influenced / Reach.

**Show the draft before writing.** The user knows what you don't — that the PR was trivial, that
someone else did the actual unblocking. Never write the ledger unattended, including on a schedule.

Push on **Unblocked** specifically. It's where most senior-IC impact lives, it leaves the least
evidence, and the user will under-report it every time. If the digests show them answering questions
in channels, that's unblocking, and it counts.

A thin week gets written thin. Don't inflate it.

## Rollup

Cluster by theme, not chronology. Three or four themes, each with its strongest cited evidence.
Lead on Influenced and Reach — shipped work is table stakes at staff level.

Then the honest pass: read `memory/context/goals.md` and name every goal with no supporting
evidence. Finding that gap in a draft is the entire point; finding it in the review is too late.

Output to `reviews/<range>-impact.md`. Draft, in the user's voice, for them to edit.

## Rules that don't bend

- Every entry cites a real artifact. No citation, no entry.
- No self-describing adjectives. State what happened.
- Name the people you unblocked.
- Shared work is described as shared.
- Nothing personal-origin, ever.
