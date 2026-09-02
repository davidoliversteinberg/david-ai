# Connectors

## How tool references work

Every file in this plugin refers to tools by **role**, written as `~~role` — never by product
name. `~~email` means "whatever email you connected." `~~tracker` means Jira, or Linear, or Asana,
or nothing at all.

This is what makes the plugin portable. Change companies, and you rewrite one file (`PROFILE.md`
in your workspace) instead of thirteen skills. Add Jira next quarter, and it's one line in
`.mcp.json` and one row in `PROFILE.md`.

The `.mcp.json` in this directory pre-configures a specific set of servers because they're the ones
that cover most people. Any MCP server that fills the role works.

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

- **`~~directory`** — usually an internal MCP exposing an org tree (`get_org_tree`,
  `users_search`, `directory_me`). Without it, `collaboration-radar` falls back to the people it
  can see on your calendar and in your threads, which is narrower but still useful.
- **`~~design`** — Figma. Without it, `system-watchtower` scans code only.

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
