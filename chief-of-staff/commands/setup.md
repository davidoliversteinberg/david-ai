---
description: Set up the chief-of-staff workspace — profile, memory, rules, task board
---

# Setup

First run. Creates the workspace, reads a month of the user's actual work to propose a profile, and
seeds the rule files.

> Placeholders like `~~email` are roles, not products — see [CONNECTORS.md](../CONNECTORS.md).

## The rule that shapes this whole command

**Ask only what no connector can know.** Everything else gets derived and shown for correction.

Someone's watch list, their tier-1 meetings, who they work with and what they own are all
reconstructable from a month of calendar and chat. Asking for them instead produces a worse answer,
because people list what they'd *say* they care about, not the four rooms they're a member of and
never read — which is exactly where the value is. Correcting a wrong row is also far cheaper than
generating an answer from a blank prompt, which matters at minute three of a setup nobody was
looking forward to.

So: two questions, then go and read, then hand back tables to correct, then ask only the handful of
things that are genuinely in the user's head.

## 1. One workspace or two

Ask this first, because it decides everything downstream and it's the one structural choice that's
annoying to reverse. Don't ask it as a preference — most people have no idea what they'd be
choosing between. Read their situation, recommend, and explain what the recommendation costs.

The framing that makes it answerable: **the question is how many workspaces, not how many
machines.** One machine can run two.

```
Before I write anything — do you want work and personal in one assistant, or two?

You've got work accounts here (M365, Jira) and no personal ones connected yet,
so my guess is you'll want this one work-only and a second workspace later for
personal. That way the impact ledger structurally cannot pick up a personal item.

The tradeoff: two workspaces never correlate, so neither one can tell you where
your actual week went. If that's the thing you want most, one workspace with
origin tagging is the better shape.
```

Read the situation from what's connected, then use this:

| Situation | Recommend | Because |
|-----------|-----------|---------|
| Employer machine, personal accounts on another | **two**, one per machine | The connectors already live apart; the boundary is physical rather than a rule. |
| One machine, employed, personal accounts on it too | **two** in two directories | Separate artifacts and a ledger that can't see personal items, for the price of running setup twice. |
| Freelance, where work and personal genuinely blend | **one** | Two workspaces need a boundary that doesn't exist in their life, and they'd re-decide it weekly. |
| Only one domain in play | **one** | Nothing to separate. |

Two things to say out loud, briefly, because people assume otherwise:

- **Two workspaces on one machine is not a security boundary.** Same login, same disk, same Full
  Disk Access grant. What it buys is separate artifacts and a structurally clean ledger. A real
  boundary means two machines.
- **The two installs won't reset each other's learning.** `memory/portable/` — how they write, what
  they care about, what they're working toward — is designed to be copied between workspaces, and
  contains no work facts by construction. Mention it now so "two" doesn't sound like starting twice.

If they're genuinely undecided, say **two**: one→two is a tedious afternoon of moving files, two→one
never merges cleanly.

Record the answer at the top of `PROFILE.md`. If they chose two, finish this workspace properly and
end by telling them exactly how to run the second — don't try to set both up in one session.

## 2. Two questions, then go

Confirm the workspace directory (default: current directory — it holds real work and personal
content, so it must **not** be inside the plugin repo). Then load
[source-adapter](../skills/source-adapter/SKILL.md), list what's connected, and confirm origins:

```
Connected here: ms365 (calendar, mail, Teams), atlassian, figma, notion.
I'll treat all of those as work. Right?

Then I'll read your last 30 days and come back with a draft — a couple of minutes.
```

Origin is the one thing worth confirming up front, because a wrong guess about which mailbox is
"work" quietly corrupts the impact ledger later. Everything else can be fixed in step 3.

Create the workspace:

```
TASKS.md  PROFILE.md  CLAUDE.md  board.html
memory/{portable/,glossary.md,people/,projects/,context/}
briefs/  reviews/  ledger/  decisions/  log/
```

Copy `board.html` from `${CLAUDE_PLUGIN_ROOT}/skills/assets/board.html` and the rest from
`${CLAUDE_PLUGIN_ROOT}/templates/`. If files already exist, don't overwrite — report what's there
and offer to update individually.

## 3. Discovery

Read, in one pass, saying what you're doing but not narrating every call:

- **Calendar, 30 days.** Recurrence, attendee count, the user's own attendance rate.
- **Chats and channels.** Enumerate everything they belong to; count messages over the window and
  count how many the user sent.
- **Tracker.** Their open issues, and the boards those issues live on.
- **Directory**, if a `~~directory` role exists. Manager, skip-level, immediate team.
- **Capabilities.** Probe transcripts and channel search once. Record the answers.

Two derivations that carry most of the value:

**Meeting tiers.** Tier 1 = recurring, small, and about a surface the user appears to own. Tier 3 =
large broadcast. Tier 2 = everything else. Attendance rate is a hint, not the answer — a meeting
they keep missing may be exactly the one to read.

**Rooms they're in but don't read.** High message volume, zero or near-zero sent by the user. These
are the highest-value, lowest-cost thing the plugin can watch, and they're the thing nobody names
when asked to list their important channels. Mark them.

## 4. Correct by exception

Hand back tables, one domain at a time, and invite corrections rather than confirmation. Never more
than two tables in one message.

```
Read your last 30 days. Here's what I think matters — fix anything wrong.

MEETINGS                          tier   why
  Design Systems Weekly             1    recurring, you're 1 of 6
  CMP/CMS Sync                      1    crosses a boundary you own
  Sprint Review                     2    you attend about half
  All Hands                         3    47 attendees

CHATS & CHANNELS               msgs/30d   you sent
  Media Library — build             412         38
  Design Systems                    206         61
  Rights & Compliance               189          0   ←
  Platform Announcements             44          0   ←

Tier 1 means I read the transcript even when you didn't attend.
The arrows are rooms you belong to and never post in — usually the most
useful thing to watch, and the ones people never think to list.

"drop 3, Sprint Review is tier 1" is a perfectly good answer.
```

Then the same shape for **surfaces** (candidates from tracker labels and recurring meeting titles —
push once for specificity, since "design system" won't match anything and "the Foundry type scale"
will) and **people** (from co-attendance and thread frequency, ten at most).

Write `PROFILE.md` and a stub `memory/projects/<slug>.md` per surface as you go, so a user who stops
here still has something that works.

## 5. The questions only a human can answer

Small batch. Everything here is genuinely unavailable to any connector.

**Bars, asked in context.** One per surviving watch-list entry, attached to the thing itself rather
than collected abstractly:

```
Rights & Compliance — 189 messages, you've posted none.
What would you actually want pulled out of there?
```

An entry that ends up without a bar goes in `## Gaps`, not in the watch list.

**Then, in one message:** the level they're aiming at and roughly when; which boundary counts as
cross-team for them (this is what `career-mentor` reasons about, and it's rarely the obvious seam on
the org chart); this quarter's goals and what done looks like; and what already lands in their inbox
that they know they don't need.

## 6. Rules

Four files in `memory/context/`, seeded from step 4 rather than asked again.

**`escalation-rules.md`** — the template defaults are already IC-shaped. Read them out, ask what's
missing. Two minutes.

**`ignore-rules.md`** — from the inbox answer above. Capture the *reason* with each rule, not just
the pattern; the reason is what tells the matcher where the boundary is.

**`goals.md`** — from the quarter answer. Mark it a draft if they didn't have it written down.

**`system-rules.md`** — ask if they own a design system and whether they have rules that are
mechanically checkable. Four or five maximum; explain that a small ruleset is deliberate. If a rule
is a personal craft principle rather than this employer's system, it belongs in
`memory/portable/craft.md` instead — see *Portable memory vs workspace memory* in
[work-memory](../skills/work-memory/SKILL.md).

**`trend-sources.md`** — optional and fine to leave empty. Say `trend-radar` won't run without
sources, which is correct behaviour rather than a gap.

Then load [work-memory](../skills/work-memory/SKILL.md) and offer to learn their shorthand from an
existing task list: *"Where do you keep your todos now? I'll read it to learn how you write rather
than guessing."* Import it, ask about unparseable terms in one batch, and write the notation to
`memory/portable/shorthand.md` — that part travels with them. Write `CLAUDE.md` from the rest.

## 7. Finish honestly, then prove it

Write `## Gaps` in `PROFILE.md` — everything setup couldn't resolve, in plain language. This is not
an admission of failure; it's what lets the sweep distinguish *nothing happened* from *nothing is
configured*, and it's what a later run reads to propose filling.

```
Workspace ready at <path>.

Still missing, and worth knowing:
  · 4 channel IDs unresolved — no name-to-ID lookup exists, so I need them from
    the Teams URL when you're next in there
  · No bar set on Platform Announcements
  · No tracker for personal work

  /chief-of-staff:brief      what's happening today
  /chief-of-staff:digest     catch up on a window
  /chief-of-staff:automate   put it on a schedule

Board's at board.html — open it from your file browser.
```

Don't run `open` — in Cowork the agent is in a VM and won't reach the user's browser.

**Then run `brief` immediately, in the same session.** This is the step that decides whether any of
the rest gets used. A brief against real data while the setup context is still warm makes wrong
tiers, missing surfaces and bad bars obvious in a way no amount of confirming a table will.

## What this command will not do

- **Won't finish.** A partial profile is the expected outcome, not a failure state. Someone who
  bails after step 3 has a working install with an honest gaps list.
- **Won't ask for anything it can read.** If a question could have been answered by a connector, it
  shouldn't have been asked.
- **Won't infer origin silently.** Everything else can be a guess shown for correction; that one
  gets confirmed out loud.
