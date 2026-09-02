# Connectors

## How tool references work

Every file in this plugin refers to tools by **role**, written as `~~role` — never by product
name. `~~email` means "whatever email you connected." `~~tracker` means Jira, or Linear, or Asana,
or nothing at all.

This is what makes the plugin portable. Change companies, and you rewrite one file (`PROFILE.md`
in your workspace) instead of thirteen skills. Add Jira next quarter, and it's one line in
`.mcp.json` and one row in `PROFILE.md`.

The `.mcp.json` in this directory is **deliberately empty**. See *Connecting sources* below for why,
and for the servers to paste in if you need them. Any MCP server that fills a role works.

## Roles

| Role | Placeholder | Servers in `.mcp.json` | Other options |
|------|-------------|------------------------|---------------|
| Calendar | `~~calendar` | Microsoft 365, Google Calendar | any calendar MCP |
| Email | `~~email` | Microsoft 365 (Outlook), Gmail | any mail MCP |
| Chat | `~~chat` | Microsoft 365 (Teams), Slack | Discord, Zulip |
| Directory | `~~directory` | — (org-published) | Entra/AD, Workday, BambooHR |
| Tracker | `~~tracker` | Atlassian (Jira) | Linear, Asana, monday.com, ClickUp |
| Docs | `~~docs` | Notion | Confluence, Guru, Coda |
| Design | `~~design` | — (org-published) | Figma |
| Code | `~~code` | — (local `git`/`gh`) | GitHub, GitLab MCP |

Two roles have no server listed because they're usually published by your employer rather than
installed by you:

- **`~~directory`** — usually an internal MCP exposing an org tree and a user search. Without it,
  `collaboration-radar` falls back to the people it can see on your calendar and in your threads,
  which is narrower but still useful. It has to say which mode it's in, every time.
- **`~~design`** — Figma. Without it, `system-watchtower` scans code only.

## Filling a role isn't all-or-nothing

A source can fill a role and still not do everything the role implies. Two that matter, both
verified against Microsoft 365 on 2026-09-02:

- **Transcripts are a sub-capability of `~~calendar`.** M365 has them — full WEBVTT with speaker
  attribution, reached via the event resource rather than the calendar search result. Other calendar
  sources may not. `meeting-digest` degrades in three tiers and says which one it's on.
- **Mail rules and blocked senders are a sub-capability of `~~email`, and M365 doesn't expose them.**
  No tool, no resource URI. `inbox-hygiene` was already designed to be advisory; for this source it
  is permanently so.

Also worth knowing: the M365 connector exposes **no send or draft tool at all**. The plugin's
never-send rule is policy everywhere else and a hard constraint here.

Probe once, record the answer under *Capabilities* in `PROFILE.md`, and don't re-probe every run.

## Connecting sources

**`.mcp.json` ships empty, on purpose.** An earlier version pre-declared six servers, and installing
the plugin in an environment that already had app-level connectors produced two of everything —
duplicate tools and a second OAuth prompt for accounts that were already authorised. Since most
people who want this plugin already have their mail and calendar connected at the app level, the
default that breaks least is to declare nothing.

So: **connect your sources however your host does it** (app-level connectors, `claude mcp add`, your
own `.mcp.json`), then record them in `PROFILE.md`. Nothing downstream cares which layer provided a
tool — the skills only ever ask for a role.

If you do need to declare servers here, these four URLs were reachable as of 2026-09-02:

```json
{
  "mcpServers": {
    "ms365":     { "type": "http", "url": "https://microsoft365.mcp.claude.com/mcp" },
    "atlassian": { "type": "http", "url": "https://mcp.atlassian.com/v1/mcp" },
    "notion":    { "type": "http", "url": "https://mcp.notion.com/mcp" },
    "slack":     { "type": "http", "url": "https://mcp.slack.com/mcp" }
  }
}
```

Two URLs that were in the original file are **wrong** and are recorded here so nobody re-adds them:
`gmail.mcp.claude.com` and `gcal.mcp.claude.com` do not resolve. Connect Gmail and Google Calendar
as app-level connectors instead.

## A role can hold more than one source

This is the part that differs from most plugins. `~~email` is a *list*, not a single server, and
each entry carries an **origin**:

| Role | Source | Origin |
|------|--------|--------|
| `~~email` | `ms365` | `work` |
| `~~email` | `gmail` | `personal` |
| `~~chat` | `ms365` (Teams) | `work` |
| `~~chat` | `slack` | `personal` |

Origin travels with every item the plugin captures — into `TASKS.md`, into memory, into briefs.
It exists so one assistant can see your whole week without personal noise leaking into
work-facing output.

Two hard rules follow from it:

- `impact-ledger` and `collaboration-radar` read **`work` origin only**. Nothing personal ever
  reaches a promo packet or a colleague-facing suggestion.
- `coaching` and the daily brief read **both**. An assistant that can only see half your week
  gives half-blind advice.

Origin is a legibility boundary, not a security one. Everything passes through the same session
and lands in the same local workspace. If that isn't acceptable for your situation, run two
workspaces with two profiles instead — the plugin supports it, it just won't correlate across them.

## Setting your roles

`/chief-of-staff:setup` writes `PROFILE.md` into your workspace. It's a plain table you can edit by
hand at any time. Templates for the common shapes are in
[`templates/`](templates/) — work-only, personal-only, and combined.

If `PROFILE.md` is missing, the plugin infers roles from whatever tools happen to be available and
tells you what it assumed. Roles with no source are skipped, not faked.
