# Profile

Which tool fills which role, and whether it's work or personal. This file is authoritative — the
plugin reads it instead of guessing. Edit it by hand whenever you add or drop a connector.

Origin is `work` or `personal`. It's decided by the **account**, not by the content. Mail from a
friend to your work address is still `work`.

| Role | Source | Origin | Notes |
|------|--------|--------|-------|
| calendar | ms365 | work | primary |
| calendar | google-calendar | personal | |
| email | ms365 | work | primary |
| email | gmail | personal | |
| chat | ms365 | work | Teams |
| chat | slack | personal | side projects |
| directory | <org connector> | work | org tree only — no message access |
| tracker | atlassian | work | Jira |
| docs | notion | work | |
| design | figma | work | |
| code | gh | work | local CLI, not MCP |

## Me

- **Name:** <your name>
- **Role:** <your title>
- **Work address:** <you@company.com>
- **Personal address:** <you@gmail.com>
- **Manager:** <name>
- **Timezone:** <IANA tz, e.g. America/New_York>

## Surfaces I own

The things you're accountable for. `collaboration-radar` intersects other people's activity against
this list, and `meeting-digest` uses it to rank what needs your attention. Be specific — "design
system" is too broad to match on; "the Foundry type scale" is matchable.

- <surface one>
- <surface two>

## Rules

- `impact-ledger` and `collaboration-radar` read **work origin only**.
- `coaching` and the daily brief read **both**.
- Anything not listed above is skipped, not approximated.
