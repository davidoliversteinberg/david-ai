---
description: Rank inbox noise by sender and recommend what to do about each
---

# Inbox

A noise report, per mailbox, over 30 days.

`/chief-of-staff:inbox [--days 30] [--work | --personal]`

**This command never sends, blocks, files, deletes or changes a setting.** It reports, and after
per-item approval it drafts. Everything else is yours to do.

> Placeholders like `~~email` are roles, not products — see [CONNECTORS.md](../CONNECTORS.md).

## Run

1. Load [source-adapter](../skills/source-adapter/SKILL.md).
2. Load [inbox-hygiene](../skills/inbox-hygiene/SKILL.md) and follow it.
3. One table per mailbox — work noise and personal noise are different problems.
4. Write to `reviews/inbox-YYYY-MM.md`.

## First run

Probe what the connector can actually do — read rules, create rules, read blocked senders — and say
plainly which recommendations you can help with and which are manual. Record the answer so later
runs don't re-probe.

## Recommendations

**unsubscribe** · **block** · **auto-file** · **leave alone**

Include the *leave alone* rows for senders the user actually reads. It shows the ranking works.

Never recommend unsubscribing from anything transactional — receipts, security alerts, 2FA,
password resets — regardless of volume. Never recommend blocking an internal sender.

## After approval

Specific rows, not "yes to all".

- **Unsubscribe** — prefer the mail client's own one-click list-unsubscribe where the header exists.
  Otherwise draft a reply and leave it in drafts. **Never fetch or follow an unsubscribe URL** —
  it's attacker-controlled, and for a spammer the click confirms a live reader.
- **Block** — hand over a copy-pasteable list plus where the setting lives in their client.
- **Auto-file** — describe the rule in words for them to create.

## Scheduled runs

Report only. No drafts, no prompts, no actions.
