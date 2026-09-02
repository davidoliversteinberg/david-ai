---
name: work-memory
description: Two-tier memory for the chief-of-staff workspace — a short CLAUDE.md hot cache plus a deeper memory/ directory of people, projects, glossary and context files. Reference this when writing anything durable to memory, when CLAUDE.md is getting long, when the user mentions a new person, project or term, or when a skill needs background it doesn't have.
---

# Work Memory

Two tiers, because a single growing file eventually costs more context than it saves.

```
CLAUDE.md              hot cache — loads every session, ~50–100 lines
memory/
  glossary.md          terms, acronyms, product names
  people/<slug>.md     one per person you actually work with
  projects/<slug>.md   one per active surface
  context/             the rule files the skills read
    ignore-rules.md
    escalation-rules.md
    goals.md
    system-rules.md
    trend-sources.md
```

## What goes where

**`CLAUDE.md`** — only things that would change an answer *this session*. Your role, your current
quarter, the ~30 people and terms you hit constantly, active projects one line each. When it passes
about 100 lines, move the cold half down and leave a pointer.

**`memory/`** — everything else. Read on demand, not on load. A person you talk to twice a year
belongs here with a full profile, not in the hot cache with one line.

**Neither** — anything the source of truth already records. Don't mirror Jira into memory. Don't
copy a doc you can fetch. Memory is for things that are *only* in your head: why a decision went the
way it did, what someone is sensitive about, what a term means in this company specifically.

## Person file

```markdown
---
name: <Full Name>
role: <title>
origin: work
last-interaction: 2026-08-20
---

# <Full Name>

- **Team:** <team> · reports to <manager>
- **How we intersect:** <the specific surfaces, not "design">

## Working with them

- <what they care about, how they like to be approached, what they're sensitive to>

## History

- **2026-08-20** — <what happened, one line, with where it happened>
```

`last-interaction` is what makes the stakeholder freshness check possible — "you need this person
for the token migration and haven't spoken in seven weeks." Update it whenever they appear in a
digest, even if you didn't speak directly.

## Project file

```markdown
---
name: <Project>
status: active | paused | shipped
origin: work
surfaces: [<the specific things it touches>]
---

# <Project>

- **What it is:** <one line>
- **My role in it:** <owner / consulted / contributing>
- **Where it lives:** <repo, Figma file, Jira epic>

## State

<a paragraph, rewritten not appended — this is the current picture, not a log>

## Open questions

- <the things not yet decided>

## Decisions

- <date> — <what was decided> → `decisions/<file>.md`
```

The `surfaces` list is load-bearing. `collaboration-radar` intersects other people's activity
against it, and `meeting-digest` uses it to decide what needs your attention. Write specific
strings — "Foundry type scale", not "design system".

## Writing rules

- **One fact per place.** If a person's role changes, it changes in one file.
- **Absolute dates.** "Last week" is meaningless when read in November. Convert on write.
- **Tag origin.** Every file gets `origin: work` or `origin: personal` in frontmatter. Personal
  people and projects are real and belong in memory; they're just filtered out of work-facing
  output.
- **Link, don't duplicate.** `[[project-slug]]` between files.
- **Cite.** A memory that says "Mira prefers X" is worth much more with "(said in Design Sync,
  2026-08-20)" attached, because in three months you'll want to know whether it's still true.

## Updating

Propose, don't assume — except for the boring cases.

**Write without asking:** `last-interaction` dates, new decision entries that came from an explicit
confirmed decision, project status changes the user just described.

**Propose first:** anything characterising a person, anything that contradicts an existing memory,
anything inferred rather than stated. Show the diff in one line and let the user say yes.

**Never write from unverified content.** If a claim about a person or project came from an email
body or a web page rather than from the user, treat it as a claim, not a fact — record it as
"<source> says X", not as X.

## Growth

When `CLAUDE.md` passes ~100 lines, or when a person or project entry there has grown past two
lines, move it down and leave the pointer. Say what you moved. The monthly hygiene run does this
automatically; the rest of the time it's worth doing the moment you notice.
