# Escalation rules

What earns an interrupt. The inverse of `ignore-rules.md`, and the more important of the two — a
brief that surfaces everything is the same as a brief that surfaces nothing.

These are tuned for a **senior individual contributor**, not a manager. A manager escalates on
"my team needs something." An IC escalates on "a thing I own is moving without me."

## Escalate

1. **Someone is blocked on me specifically.** Not "the team is blocked" — me, by name, waiting on
   a review, a decision, a file, an answer.

2. **A decision is being made about something I own, and I'm not in the room.** This outranks a
   direct @-mention about scheduling. Match against the *surfaces I own* list in `PROFILE.md`.

3. **My design system is being violated, forked, or hardcoded around.** Someone shipping a
   one-off token, copying a component instead of importing it, or building a parallel version of
   something that exists.

4. **A skip-level or exec names my area.** Even in passing. Especially in passing.

5. **Something I committed to is due inside 48 hours.** From `TASKS.md`, with the source.

6. **A commitment made *to* me has gone quiet past its date.** The `@waiting` items that stopped
   moving.

## Rank by

When more than three things qualify, order by: *blocking someone else* → *decision without me* →
*deadline inside 48h* → *system violation* → *exec attention* → *stale waiting*.

Never surface more than five. If there are more than five, the sixth onward go into a
"also, lower priority" one-liner.

## Don't escalate

- FYI @-mentions in channels where I'm not named in the ask.
- Anything I've already replied to or reacted to. **Check this before surfacing** — re-surfacing
  something already handled is the fastest way to lose trust in the brief.
- Anything already on my calendar to discuss in the next 48 hours.
- Anything caught by `ignore-rules.md`, unless it also matches "Never ignore" there.

## Evidence

Every escalated line carries a verbatim quote and a link to the actual message. No paraphrase-only
escalations — if you can't quote it, you can't escalate it.
