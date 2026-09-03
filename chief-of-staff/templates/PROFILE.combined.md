# Profile

Which tool fills which role, what's worth watching, and what this install still doesn't know. This
file is authoritative — the plugin reads it instead of guessing. Edit it by hand whenever you add or
drop a connector.

Origin is `work` or `personal`. It's decided by the **account**, not by the content, and not by the
machine. Mail from a friend to your work address is still `work`. A work mailbox mirrored into Apple
Mail on your personal laptop is also still `work`.

## Sources

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

The `Notes` column also carries **exclusions** — `excluded: <skill>, <skill>` means those skills skip
this source regardless of its origin. See *Device-local sources* in `CONNECTORS.md`.

## Capabilities

Roles decompose into capabilities, and a source can fill a role without doing everything the role
implies. Probe once, record here, don't re-probe every run. Absent from this table means
**unknown**, not **no**.

| Capability | Source | Answer | Verified | Consequence |
|------------|--------|--------|----------|-------------|
| `calendar.transcript` | ms365 | yes — two-hop via the event resource | 2026-09-02 | `meeting-digest` runs at tier 1 |
| `chat.channels` | ms365 | yes, but only with no date filter | 2026-09-02 | filter by date after the call, not in it |
| `mail.rules` | ms365 | no | 2026-09-02 | `inbox-hygiene` is advisory for this source |
| `*.send` | ms365 | no tool exists | 2026-09-02 | never-send is connector-enforced here |

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

## Meetings

Tiers decide what gets read. **Tier 1 is read even when you didn't attend** — that's the whole point
of the tier, and it's how you stay current on a room you can't always be in.

| Meeting | Tier | Why |
|---------|------|-----|
| <recurring meeting> | 1 | <you own a surface it decides on> |
| <recurring meeting> | 2 | <read it when you attended> |
| <all-hands> | 3 | <skip unless a standing query hits it> |

## Watch list

`ambient-sweep` reads this and nothing else. **The sweep is exactly as good as this list** — an empty
list is not a quiet week, it's an unconfigured one, and the sweep will say so rather than reporting
silence.

**Every entry needs a bar**: the thing that would make an item worth interrupting you for. An entry
without a bar degrades into noise within two weeks and gets muted, taking the useful entries with
it. A bar is usually easier to write as an exclusion — *"release notes and permission changes, not
outage reports"* beats *"important platform news."*

### Channels

| ID | Label | Why watched | Bar |
|----|-------|-------------|-----|
| <channel id> | <label> | <why> | <what earns an interrupt> |

Channel IDs can't be looked up from a name — capture them when you have them.

### Chats

| ID | Label | Why watched | Bar |
|----|-------|-------------|-----|
| <chat id> | <label> | <why> | <what earns an interrupt> |

### Standing queries

Terms swept every run.

- <term>

## Gaps

What this install knows it doesn't know. Setup writes this honestly rather than pretending it
finished; skills read it; the sweep reports on it instead of failing quietly. Delete a line when you
fill it in.

- <e.g. "3 watched channels have no bar set">
- <e.g. "no tier-1 meetings — the sweep won't read any transcript unattended">
- <e.g. "12 channel IDs unresolved">

## Rules

- `impact-ledger`, `collaboration-radar` and `career-mentor` read **work origin only**.
- `coaching` and the daily brief read **both**.
- A source can also be excluded per-skill in the `Notes` column, independently of origin.
- Anything not listed above is skipped, not approximated.
