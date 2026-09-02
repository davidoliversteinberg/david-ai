---
name: impact-ledger
description: Keeps a weekly cited record of what you shipped, who you unblocked, which decisions you influenced and how far your reach extended, and rolls it up into a promotion case on demand. Reads work sources only. Reference this for the weekly review, when the user asks what they got done, or when preparing for a performance review, promo packet or self-assessment.
---

# Impact Ledger

Nobody remembers their own scope at review time. Six months of work compresses into "I did the
design system stuff" and the case gets made from whatever happened in the last three weeks.

This is the highest-leverage thing in the plugin for a senior IC, and the value compounds — a ledger
started today is worth ten times more in March than one started in March.

**Reads `work` origin only.** Filter at gather time. A personal item in this file eventually ends up
in a document you hand to your manager.

Load [source-adapter](../source-adapter/SKILL.md) first.

## The four categories

For a staff-level IC these map onto how promotion cases are actually argued. Scope and influence,
not output volume.

**Shipped** — what landed. Merged PRs, published Figma libraries, documents that got adopted,
features that went live. Needs a link to the artifact.

**Unblocked** — who you got moving. This is where most IC impact hides and where almost nobody keeps
records. A twenty-minute conversation that unstuck a team for a week is worth more than a day of
your own output, and it leaves no trace unless you write it down. Needs the person, the thing they
were stuck on, and where it happened.

**Influenced** — decisions that went your way, and where. Not "I advocated for X" but "X was
decided, here's the thread, here's my argument in it." The distinction between advocating and
influencing is the whole ballgame at staff level.

**Reach** — teams and surfaces you touched beyond your own. Who consulted you. What you reviewed
that wasn't yours. Where your work got adopted by someone you don't work with.

## Entry format

Append to `ledger/YYYY-Www.md`, one file per ISO week.

```markdown
# 2026-W36 · Aug 31 – Sep 4

## Shipped
- **Type scale migration, phase 2** — 14 components moved off hardcoded sizes.
  [PR #482](<url>) · merged Sep 3

## Unblocked
- **Maya Chen** — stuck on which token to use for inline links; she'd shipped `fg.success` in two
  places. Answered in #design-system Sep 2, she shipped the fix same day. [thread](<url>)

## Influenced
- **Density spec will ship behind a flag, not as a breaking change.** Argued in the Platform Sync
  Sep 1 that a hard cutover would break three teams mid-quarter. Decision recorded in
  `decisions/2026-09-01-density-flag.md`.

## Reach
- Reviewed onboarding flow for the Growth team (not my area) at Priya's request — Sep 2.
- Type scale doc cited by the Content team in their style guide update.
```

## Evidence rules

The rules are the product. A ledger of self-reported adjectives is worthless in a review; a ledger
of cited facts is the strongest thing you can bring.

- **Every entry cites a real artifact.** A link, a thread, a PR, a document, a dated meeting. No
  citation, no entry.
- **No adjectives about yourself.** "Led," "drove," "spearheaded" — cut them. State what happened
  and let the facts carry it. The strongest entries read almost flat.
- **Name the other person.** "Unblocked a teammate" is unverifiable. "Unblocked Maya on X" is
  checkable, which is exactly why it's credible.
- **Record the outcome, not the effort.** "Spent three days on the migration" is not impact. "14
  components migrated" is.
- **Don't claim shared work as solo.** If three people shipped it, say so and say what you did. Over-
  claiming is the fastest way to lose a promo case in calibration.
- **Uncertain attribution goes in with a question mark.** If a decision went your way and you're not
  sure your argument caused it, write it that way. You can firm it up later.

## Weekly run

1. Read the week's digests from `briefs/`, plus `decisions/` entries, plus tasks that moved to
   `@done` in `TASKS.md`.
2. Pull `~~code` merges and `~~tracker` closures for the week if available.
3. Draft entries under the four headings.
4. **Show the draft.** The user knows things you don't — that the PR was trivial, that the
   unblocking conversation was actually someone else's. Never write the ledger unattended.
5. Write on confirmation.

A thin week is a real result. Write it thin. A ledger that shows four heavy weeks and one light one
is more believable than five identical ones, and the light weeks are themselves information — that's
what `coaching` reads to notice your time went somewhere it shouldn't have.

## Roll-up

On request — "promo packet", "self-review", "brag doc", "what did I do this half":

1. Read every ledger file in the range.
2. Cluster by **theme**, not by week. Nobody reads a chronology. Three or four themes, each one a
   narrative: what the situation was, what you did, what changed.
3. Lead each theme with its strongest cited evidence.
4. Foreground **Influenced** and **Reach**. Shipped is table stakes at staff level; scope of
   influence is the actual argument.
5. Flag the gaps honestly — if `memory/context/goals.md` names a target with no supporting entries,
   say so. Better to find out in a draft than in the review.
6. Output to `reviews/<range>-impact.md`. Draft only, in the user's voice.

## Origin check

Before writing anything: confirm every entry traces to a `work`-origin source. If an item's origin
is unclear, leave it out and say you did.
