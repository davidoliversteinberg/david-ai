# Profile

Personal only — Google account, no employer. Useful for freelancers, for job searches, and as the
fresh-clone test case: with `directory`, `tracker` and `chat` all absent, every skill has to degrade
without erroring.

| Role | Source | Origin | Notes |
|------|--------|--------|-------|
| calendar | google-calendar | personal | |
| email | gmail | personal | |

## Me

- **Name:** <your name>
- **Personal address:** <you@gmail.com>
- **Timezone:** <IANA tz>

## Surfaces I own

- <what you're working on>

## What this shape means

- `impact-ledger` and `collaboration-radar` read work origin only, so with no work sources they
  produce **nothing**. That's correct behaviour, and they should say "no work sources configured"
  rather than falling back to personal data.
- `meeting-digest`, `commitment-capture`, `signal-filter`, `coaching`, `inbox-hygiene` and
  `trend-radar` all work fine.
- `system-watchtower` works if you point it at a local repo — it reads `~~code`, not a work account.
