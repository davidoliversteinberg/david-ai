# Ignore rules

Things that should never reach a brief. Written in your own words — the plugin reads them as
instructions, not as regexes.

Add one by just saying "ignore those" after something useless shows up. The rule gets written here
with the date and the reason.

**Nothing is deleted.** Every suppressed item is appended to `log/ignored-YYYY-MM-DD.md` with the
rule that caught it. If a rule turns out to be too broad, the log is how you find out.

## Rules

<!-- Format: one bullet per rule. State the trigger and the reason. The reason matters —
     it's what lets the rule generalize correctly to items that don't match word-for-word. -->

- **Example — Figma seat requests.** Emails about Figma seat approvals can be ignored. I'm an admin
  on our Figma instance but Tom handles all seat approvals. *(added 2026-09-02)*

## Never ignore

Overrides. If an item matches both an ignore rule and something here, it surfaces.

- Anything from my manager or skip-level.
- Anything where I'm the only person named.
- Anything with a legal, security, or incident label.

## Review

When a rule hasn't caught anything in 60 days, mention it — either the noise stopped or the rule is
mis-worded.
