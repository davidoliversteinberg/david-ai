---
description: Set up the chief-of-staff workspace — profile, memory, rules, task board
---

# Setup

First run. Creates the workspace, works out which tools fill which role, and seeds the rule files.

> Placeholders like `~~email` are roles, not products — see [CONNECTORS.md](../CONNECTORS.md).

Aim for about ten minutes of conversation, not a form. Ask in batches and accept "skip" for
anything.

## 1. Where

Confirm the workspace directory. Default is the current working directory. It holds real work and
personal content, so it must **not** be inside the plugin repo.

Create:

```
TASKS.md  PROFILE.md  CLAUDE.md  board.html
memory/{glossary.md,people/,projects/,context/}
briefs/  reviews/  ledger/  decisions/  log/
```

Copy `board.html` from `${CLAUDE_PLUGIN_ROOT}/skills/assets/board.html`. Copy the rest from
`${CLAUDE_PLUGIN_ROOT}/templates/`.

If the directory already has these, don't overwrite — report what's there and offer to update
individual files.

## 2. Profile

Load [source-adapter](../skills/source-adapter/SKILL.md).

List what's actually connected. Sort by role. Show the user the inferred table and have them correct
it — especially the origin column, since that's the one that's guessable and consequential.

```
Here's what I can see:

  calendar   ms365            work       ✓
  email      ms365            work       ✓
  chat       ms365 (Teams)    work       ✓
  email      gmail            personal   ✓
  tracker    —                           not connected
  design     —                           not connected

Right? Anything mislabelled?
```

Then ask for the short identity block — name, role, work address, personal address, manager,
timezone. Write `PROFILE.md`.

Pick the closest template in `${CLAUDE_PLUGIN_ROOT}/templates/` as the starting shape:
`PROFILE.combined.md`, `PROFILE.work-ms365.md`, or `PROFILE.personal-google.md`.

## 3. Surfaces

The most important question in setup, and the one to spend time on.

```
What are you actually accountable for? Be specific — "the Foundry type scale"
is matchable, "design system" isn't. Three to six of them.
```

These drive ranking in the digest and overlap detection in the radar. Vague surfaces produce a
useless radar. Push once for specificity if the first answer is broad.

Write them to `PROFILE.md` and create a stub `memory/projects/<slug>.md` for each.

## 4. Memory bootstrap

Load [work-memory](../skills/work-memory/SKILL.md).

Best source of real shorthand is an existing task list. Ask:

```
Where do you keep your todos now? A file, an app, a notes doc — I'll read it to
learn your shorthand rather than guessing at it.
```

Import what's there. For each term you can't parse, ask — in one batch, not one at a time.

Then, if a `~~directory` role exists, pull the org tree around the user and propose `memory/people/`
files for their manager, skip-level, and the handful of people they work with most. Don't create
fifty. Ten at most, and let the rest accumulate naturally.

Write `CLAUDE.md` from what you learned.

## 5. Rules

Four files in `memory/context/`. Copy the templates, then fill each one with a short conversation.

**`escalation-rules.md`** — the template's defaults are already IC-shaped. Read them out, ask what's
missing. Two minutes.

**`ignore-rules.md`** — ask: *"What lands in your inbox that you already know you don't need to
see?"* Capture the reason with each rule, not just the pattern.

**`goals.md`** — ask for the quarter's goals and what "done" looks like for each. If they don't have
them written anywhere, three questions is enough to draft something; mark it as a draft.

**`system-rules.md`** — ask if they own a design system and whether they have rules that are
mechanically checkable. If the user has existing craft notes anywhere, offer to seed from those.
Four or five rules maximum. Explain that a small ruleset is a deliberate choice.

**`trend-sources.md`** — optional, and fine to leave empty. Say `trend-radar` won't run until it has
sources, which is correct behaviour rather than a gap.

## 6. Finish

```
Workspace ready at <path>.

  /chief-of-staff:brief      what's happening today
  /chief-of-staff:digest     catch up on a window
  /chief-of-staff:automate   put it on a schedule

Board's at board.html — open it from your file browser.
```

Don't run `open` — in Cowork the agent is in a VM and it won't reach the user's browser.

Suggest running `brief` immediately so the first output happens while the setup context is fresh and
mistakes are easy to spot.
