---
name: source-adapter
description: Resolves which connected tool fills each role (calendar, email, chat, directory, tracker, docs, design, code) and tags every item with a work or personal origin. Every other chief-of-staff skill loads this first. Reference this when a skill needs to gather from a source, when a role appears to be missing, or when the user changes jobs, adds a connector, or asks why something was skipped.
---

# Source Adapter

Every other skill in this plugin talks in **roles**. This skill turns a role into actual tool calls.

Load it before gathering anything. It is the only place in the plugin that knows product names.

## The roles

| Role | What it answers |
|------|-----------------|
| `~~calendar` | What meetings happened or are coming |
| `~~email` | What landed in the inbox |
| `~~chat` | What was said in channels and DMs |
| `~~directory` | Who people are and who they report to |
| `~~tracker` | What tickets exist and who owns them |
| `~~docs` | What's written down |
| `~~design` | What's being designed |
| `~~code` | What's shipping |

## Resolution order

**1. Read `PROFILE.md` from the workspace.** This is authoritative. If it exists, use it and don't
second-guess it.

```markdown
| Role | Source | Origin | Notes |
|------|--------|--------|-------|
| calendar | ms365 | work | primary |
| calendar | google-calendar | personal | |
| email | ms365 | work | primary |
| email | gmail | personal | |
| chat | ms365 | work | Teams |
| chat | slack | personal | side projects |
| directory | opti | work | org tree only, no messages |
| tracker | atlassian | work | |
```

**2. If `PROFILE.md` is missing, infer.** List the available tools and sort them by role using the
table below. Then *tell the user what you assumed* in one line before you use it, and offer to write
it to `PROFILE.md`. Never infer silently — a wrong guess about which mailbox is "work" corrupts the
impact ledger quietly.

| Tool prefix | Role(s) | Default origin |
|-------------|---------|----------------|
| `ms365`, `outlook`, `microsoft` | calendar, email, chat | work |
| `gmail` | email | personal |
| `google-calendar`, `gcal` | calendar | personal |
| `slack` | chat | ask — could be either |
| `atlassian`, `jira`, `linear`, `asana`, `monday`, `clickup` | tracker | work |
| `notion`, `confluence`, `guru`, `coda` | docs | work |
| `figma` | design | work |
| `git`, `gh`, `github`, `gitlab` | code | work |
| org-published (`org_*`, `directory_me`, anything with an org tree) | directory | work |

Slack is genuinely ambiguous — plenty of people have a work Slack *and* a personal one. If Slack is
the only chat source and origin is unset, ask once and write the answer to `PROFILE.md`.

**3. If a role has no source, skip it.** Don't fabricate, don't approximate, don't apologise at
length. Say what you skipped in one line at the end of the output:

> Skipped: tracker (no tracker connected), design (no Figma).

In an interactive session only, you may additionally surface a connector suggestion. Never do this
in a scheduled run — nobody is there to click it.

## Origin tagging

**Every item you gather carries its origin from the moment you read it.** Not at write time — at
read time. If you gather 40 emails from two mailboxes and only tag them when writing to `TASKS.md`,
you will get it wrong.

Origin is `work` or `personal`. It comes from the `PROFILE.md` row that produced the item, not from
the item's content. An email from your sister to your work address is still `work` origin — the
mailbox decides, not the sender.

Carry it into:

- `TASKS.md` — as a trailing tag on the source sub-bullet
- `memory/` — in the frontmatter of anything written
- briefs and reports — as a section boundary

### Who reads what

| Consumer | Reads |
|----------|-------|
| `meeting-digest`, daily brief | both, sectioned |
| `commitment-capture` | both, tagged |
| `signal-filter` | both |
| `coaching` | both |
| `inbox-hygiene` | both, but reports separately per mailbox |
| **`impact-ledger`** | **`work` only** |
| **`collaboration-radar`** | **`work` only** |
| `system-watchtower`, `trend-radar` | neither — these read code, design and the web |

The two bolded rules are not preferences. A personal item in the impact ledger ends up in a promo
packet. A personal item in the collaboration radar ends up in a message to a colleague. Enforce
them at gather time by filtering the source list before the first tool call, not by filtering
results afterwards.

## Gathering

Once roles are resolved:

- **Query each source in a role separately, then merge.** Two mailboxes are two queries.
- **Deduplicate across sources.** A meeting on both your work and personal calendar is one meeting.
  Match on start time + title similarity, keep the work-origin copy, note the duplication.
- **Respect the window.** Skills pass a window (`today`, `--since 7d`). Apply it per source; some
  APIs want a date range, some want a count.
- **Failure is per-source, not fatal.** If Gmail times out and Outlook doesn't, produce the report
  with a line saying Gmail was unreachable. Never emit a brief that silently omits half your week.
- **Prefer one broad call over many narrow ones** where the API allows it. Ten targeted searches
  cost ten round trips and usually return the same items.

## Changing jobs

The portability promise. When the user starts somewhere new:

1. Update `.mcp.json` if the new company uses different servers.
2. Rewrite `PROFILE.md` — new sources, same roles.
3. Archive the old workspace directory; start a fresh one.
4. Nothing in `skills/` changes.

If a skill ever needs editing to accommodate a new employer, that's a bug in the skill — it means a
product name leaked out of this file.
