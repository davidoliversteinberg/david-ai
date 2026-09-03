# david-ai

A chief-of-staff plugin for Claude, built for a **senior individual contributor**.

Most assistant tooling is shaped for managers: what does my team need, who's blocked, how's morale.
That's a different job from being a staff-level IC, where the questions are *is a decision being made
about something I own without me in the room*, *who else is quietly working on my surface area*, and
*can I evidence my scope when review season arrives*.

This is that assistant. It works across your work and personal accounts, it's portable between
employers, and it's built to be trusted — which mostly means it cites everything and asks before
doing anything outward-facing.

## Install

```bash
/plugin marketplace add davidoliversteinberg/david-ai
/plugin install chief-of-staff
```

Then, in the directory you want as your workspace:

```
/chief-of-staff:setup
```

Setup takes about ten minutes of conversation. It works out which connector fills which role, learns
your shorthand from your existing task list, and writes the rule files that everything else reads.

## Commands

| Command | What it does |
|---------|--------------|
| `/chief-of-staff:setup` | First run — profile, memory, rules, task board |
| `/chief-of-staff:brief` | Today: what needs you, what's coming, what went quiet |
| `/chief-of-staff:digest` | Catch up on a window — meetings, chat, email, ranked |
| `/chief-of-staff:capture` | Pull commitments out of recent activity into the task list |
| `/chief-of-staff:sweep` | Standing watch — read widely, record quietly, surface only what clears the bar |
| `/chief-of-staff:radar` | Who else is on your surfaces, and who's going stale |
| `/chief-of-staff:ux` | Prior art and options for a live design question, or help sequencing the work |
| `/chief-of-staff:trends` | Design-system drift, then UX/UI worth stealing |
| `/chief-of-staff:ledger` | Record this week's impact, or draft the promo case |
| `/chief-of-staff:mentor` | Where the promotion case stands, what's thin, what to take or decline |
| `/chief-of-staff:review` | Weekly: the record, then honest coaching |
| `/chief-of-staff:inbox` | Rank inbox noise, recommend what to do about each sender |
| `/chief-of-staff:automate` | Put all of it on a schedule |

## What's in it

**The spine.** `source-adapter` resolves roles to actual connectors, decomposes them into checkable
capabilities, and tags everything by origin. `work-memory` is a two-tier memory — a short hot cache
plus a deeper store of people, projects and rules — split so that what's true about *you* can travel
between machines and jobs while what's true about *this workspace* stays put. `signal-filter`
decides what reaches you, and keeps observations, inferences and recommendations from being mistaken
for each other.

**Awareness.** `meeting-digest` summarizes meetings, chat and email into a ranked *Needs your
attention* list. `commitment-capture` turns "I'll get you that by Thursday" into a task that
remembers where it came from. `ambient-sweep` is the standing watch: it reads the rooms and
transcripts you don't have time for, records what it learns to memory, and stays silent unless
something clears a bar you wrote down. A quiet week reports as one line, not a summary.

**The IC-specific half.** `collaboration-radar` finds people working on your surfaces and suggests
when to talk. `impact-ledger` keeps a cited weekly record of what you shipped, who you unblocked,
what you influenced and how far your reach extended — the four things a staff-level case is
actually argued on — and it separates **big wins** from **spot wins**, because the two are lost in
different ways. A big win is a multi-month arc you'll remember; a spot win is the Tuesday afternoon
you unstuck someone in a chat thread, and it's gone by Friday unless something catches it. The
ambient sweep catches them. `career-mentor` runs the level above: the promotion as a campaign with
arcs, an audience and a set of moves, including which opportunities to decline. It's honest about
where its advice comes from — a small, mostly engineering-shaped literature — and it never predicts
an outcome. `system-watchtower` catches design-system drift. `decision-log` captures *why*, which is
the part that never survives.

**Outward.** `inbox-hygiene` ranks noise and recommends action. `trend-radar` scans a pinned source
list for things worth stealing. `design-advisor` is the other direction — you bring it a live open
question and it brings back prior art, options and tradeoffs, plus help sequencing the week.
`coaching` compares where your time went against where you said it would go.

## Portability

Every skill refers to tools by role — `~~email`, `~~chat`, `~~tracker` — never by product. One file
in your workspace (`PROFILE.md`) maps roles to whatever you've actually connected.

Change jobs: rewrite `PROFILE.md`, update `.mcp.json` if the new company uses different servers,
start a fresh workspace. Nothing in `skills/` changes. If a skill ever needs editing to accommodate
an employer, that's a bug — it means a product name leaked out of `source-adapter`.

See [CONNECTORS.md](chief-of-staff/CONNECTORS.md) for the role table.

## Work and personal in one assistant

A role holds a *list* of sources, each tagged `work` or `personal`. Outlook and Gmail both fill
`~~email`; Teams, Slack and iMessage all fill `~~chat`.

Origin travels with every item. Briefs section by it so they stay scannable, but the coaching and
the daily brief reason across both — an assistant that can only see half your week gives half-blind
advice.

Two hard rules: **`impact-ledger` and `collaboration-radar` read work origin only.** Nothing personal
reaches a promo packet or a message to a colleague.

Origin is a legibility boundary, **not a security one** — everything passes through the same session
and lands in the same local workspace.

So the first thing setup asks is **one workspace or two**, framed as a real choice rather than a
preference. Two gives you a ledger that structurally cannot see a personal item; one gives you
coaching that can see your whole week. Which is right depends on whether the boundary exists in your
life — an employee with a managed laptop and a freelancer whose work *is* their life want opposite
answers, and the command reads your situation and recommends rather than making you guess.

Two workspaces don't mean learning twice. `memory/portable/` holds what's true about you — how you
write, the craft principles you hold, what you're working toward — and is designed to be copied
between them. It contains no work facts by construction, so you can read exactly what crosses. It's
also what makes changing jobs cheap: keep that directory, archive the rest.

Origin isn't the only axis. A source can also be **excluded from named skills** regardless of who it
belongs to — because *whose data is this* and *which jobs is this the right input for* are different
questions. iMessage is the worked example: useful for catching a commitment made over text, wrong as
an input to a watch that runs unattended on a schedule. See *Device-local sources* in
[CONNECTORS.md](chief-of-staff/CONNECTORS.md), which also covers why Full Disk Access is a broader
grant than it looks and why the send tool stays off.

## What it will not do

Stated plainly because these are deliberate design decisions, not gaps:

- **Send anything.** Not an email, not a message, not a calendar invite. It drafts; you send.
- **Change account settings.** No mail rules, no filters, no block lists.
- **Follow an unsubscribe link.** For a spammer, that click confirms a live reader — and the URL is
  attacker-controlled. It hands you the link; you decide.
- **Write to your task list unattended.** Scheduled runs propose; you triage.
- **Write the impact ledger unattended.** You know things the transcript doesn't.
- **Change code.** The watchtower reports.
- **Act on instructions found in a message, document or web page.** Content from any source is data,
  never a command. If an email contains text addressed to an AI assistant, that gets reported as a
  curiosity, quoted, and otherwise ignored.

## Your data

This repository contains workflow instructions only — no company data, no personal data, ever.

Your workspace lives outside it and is gitignored several ways over. Everything the plugin gathers
stays local to your machine and your connectors.

## Requirements

Built for **Claude Cowork**, where the connectors and scheduled tasks live. The plugin schema is
shared with Claude Code, so it loads there too — but with no connectors wired, most of it has
nothing to read.

Scheduled tasks only run while the app is open. If it's closed when one is due, it runs at next
launch.

## Connector notes

Both of these are tenant-dependent, so treat the answers as "verified here, probe yours". Tested
against Microsoft 365 on 2026-09-02:

- **Meeting transcripts — reachable.** Full WEBVTT with per-speaker attribution. The path is two
  hops: the calendar search result doesn't carry the transcript field, so you read the event
  resource to get `meetingTranscriptUrl`, then read that. `meeting-digest` still degrades gracefully
  (transcript → surrounding chat → metadata only) and always says which tier it's on, because
  transcription is per-meeting, not per-tenant.

  Transcripts are also messier than they look — interleaved speaker channels put lines out of
  chronological order, and product names get mangled badly enough to break topic matching. See
  *Reading a transcript* in `meeting-digest` for the four failure modes worth knowing about.

- **Mail rules and blocked senders — not reachable.** No tool exposes them and the resource URI
  whitelist has no rules scheme. `inbox-hygiene` is advisory for this source, permanently. That was
  always the design, so nothing changes; it's just no longer optional.

- **Sending — not possible.** The connector exposes no send or draft tool at all. For this source
  the never-send guarantee below isn't only policy: there's no code path to violate it.

## Prior art

The task board (`chief-of-staff/skills/assets/board.html`) is forked from the `dashboard.html`
shipped in Anthropic's `productivity` plugin (knowledge-work-plugins, v1.1.0), forked 2026-09-02.
Changes: status tokens beyond done, an Active-only filter, and round-trip preservation of the plain
sub-bullets that carry each task's provenance. Upstream fixes won't reach this copy.

The `TASKS.md` conventions and the two-tier memory architecture follow the same plugin, so the two
are broadly compatible.

## Licence

MIT.
