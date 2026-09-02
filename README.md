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
| `/chief-of-staff:radar` | Who else is on your surfaces, and who's going stale |
| `/chief-of-staff:trends` | Design-system drift, then UX/UI worth stealing |
| `/chief-of-staff:ledger` | Record this week's impact, or draft the promo case |
| `/chief-of-staff:review` | Weekly: the record, then honest coaching |
| `/chief-of-staff:inbox` | Rank inbox noise, recommend what to do about each sender |
| `/chief-of-staff:automate` | Put all of it on a schedule |

## What's in it

**The spine.** `source-adapter` resolves roles to actual connectors and tags everything by origin.
`work-memory` is a two-tier memory — a short hot cache plus a deeper store of people, projects and
rules. `signal-filter` decides what reaches you.

**Awareness.** `meeting-digest` summarizes meetings, chat and email into a ranked *Needs your
attention* list. `commitment-capture` turns "I'll get you that by Thursday" into a task that
remembers where it came from.

**The IC-specific half.** `collaboration-radar` finds people working on your surfaces and suggests
when to talk. `impact-ledger` keeps a cited weekly record of what you shipped, who you unblocked,
what you influenced and how far your reach extended — the four things a staff-level case is
actually argued on. `system-watchtower` catches design-system drift. `decision-log` captures *why*,
which is the part that never survives.

**Outward.** `inbox-hygiene` ranks noise and recommends action. `trend-radar` scans a pinned source
list for things worth stealing. `coaching` compares where your time went against where you said it
would go.

## Portability

Every skill refers to tools by role — `~~email`, `~~chat`, `~~tracker` — never by product. One file
in your workspace (`PROFILE.md`) maps roles to whatever you've actually connected.

Change jobs: rewrite `PROFILE.md`, update `.mcp.json` if the new company uses different servers,
start a fresh workspace. Nothing in `skills/` changes. If a skill ever needs editing to accommodate
an employer, that's a bug — it means a product name leaked out of `source-adapter`.

See [CONNECTORS.md](chief-of-staff/CONNECTORS.md) for the role table.

## Work and personal in one assistant

A role holds a *list* of sources, each tagged `work` or `personal`. Outlook and Gmail both fill
`~~email`; Teams and Slack both fill `~~chat`.

Origin travels with every item. Briefs section by it so they stay scannable, but the coaching and
the daily brief reason across both — an assistant that can only see half your week gives half-blind
advice.

Two hard rules: **`impact-ledger` and `collaboration-radar` read work origin only.** Nothing personal
reaches a promo packet or a message to a colleague.

Origin is a legibility boundary, **not a security one** — everything passes through the same session
and lands in the same local workspace. If that's not acceptable for your situation, run two
workspaces with two profiles.

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
