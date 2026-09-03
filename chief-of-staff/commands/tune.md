# Tune

Grade the assistant's own week and propose changes to its rules.

Every other command reports outward. This one is the only path by which being wrong on Tuesday
changes anything on Wednesday.

> Placeholders like `~~chat` are roles, not products — see [CONNECTORS.md](../CONNECTORS.md).

## Run

1. Load [signal-filter](../skills/signal-filter/SKILL.md) — the rules you are proposing to change.
2. Read the signals, strongest first:

   | Signal | Where | Strength |
   |---|---|---|
   | Corrections the user typed | `feedback.md` | **decisive** — one line is enough |
   | Proposed vs merged | `proposals/*.md` against `TASKS.md` | needs a pattern |
   | Escalations never acted on | `briefs/*.md` **Needs you**, vs whether they ever replied | needs a pattern |
   | Suppressed but mattered | `log/ignored-*.md` | **expensive** — invisible by design |

3. Write `proposals/rules-YYYY-Www.md`.

## Reading the weak signals

A dropped proposal is not proof of a bad rule. They may have been busy, done it without ticking it,
or judged it correctly captured and not worth tracking. **One drop is noise. The same category
dropped three times is signal.** Say which one you are looking at.

Never propose a change off a single data point unless it came from `feedback.md`, where one line is
enough because they wrote it deliberately.

Where the evidence is ambiguous, write the **open question** instead of inventing a rule. *"Four
calendar-logistics items were proposed and none merged — are those worth capturing at all?"* is more
useful than a confidently wrong rule, and costs the user one line to answer.

## Format

```
## Proposed rule changes
  – the exact text to paste, and which file it belongs in
  – the evidence, cited
  – the reason, in their words where you have them

## Open questions
  – one question each, answerable in a line

## Health check
  – rules that caught nothing in 60 days
  – rules suppressing >20 items/week, with three examples to confirm

## Coverage
  – which signals existed this run
```

Every rule carries a **reason**, because the reason is what tells a future run where the boundary
is. A rule without one generalizes wrongly the first time it meets a case its author didn't imagine.

## Propose, never apply

Write the proposals file. Do not edit `ignore-rules.md`, `escalation-rules.md`, `CLAUDE.md` or
`PROFILE.md` — not even the changes you are most confident about.

An assistant that quietly rewrites its own filters cannot be audited, and the reason the user trusts
a brief is that they can always find out why something did or didn't surface. That property is worth
more than the convenience of applying a rule automatically.

## An empty week

If `feedback.md` has nothing new and nothing was merged from `proposals/`, there was no signal. Say
so in one line and stop. Manufacturing findings from an empty week is how this command becomes the
one they skip.
