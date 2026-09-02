---
name: system-watchtower
description: Scans code and design files for violations of the user's own design-system rules and reports each one with the rule it breaks and where. Reference this for the weekly craft run, when the user asks whether anything is drifting, or when reviewing UI that someone else (or an agent) built.
---

# System Watchtower

Design systems don't fail loudly. They fail through a hundred small local decisions — someone
hardcodes a size because the token didn't exist yet, an agent generates a component that copies
values instead of importing them, a one-off gets copied twice. By the time it's visible it's
expensive.

This watches for the specific violations you've decided to care about.

## Rules come from the user

`memory/context/system-rules.md` is the whole ruleset. This skill has no built-in opinions about
design — it enforces what's written there.

Every rule needs a **Detect** line. A rule you can't mechanically look for is a principle, and
principles belong in `CLAUDE.md` where they inform judgement, not here where they'd produce
guesswork.

Start narrow. Four or five rules on one repo. A watchtower that reports twenty things a week gets
muted in a fortnight; one that reports two real things a month gets read every time.

## What it scans

- **`~~code`** — diffs, PRs and recently changed files in the repos named under each rule's
  *Applies to*. Prefer the diff over the whole tree: you want new drift, not the historical backlog.
- **`~~design`** — recently edited files, if a design connector exists. Layer properties, detached
  components, local styles where a library style exists.

If a rule names a repo or file the scan can't reach, **say so and skip the rule**. Reporting "all
clear" on something you couldn't read is the one genuinely damaging failure mode here.

## Baseline

First run establishes a baseline and **doesn't report it**. An existing codebase will have hundreds
of pre-existing violations and dumping them is useless.

Write the count to `reviews/watchtower-baseline.md` — "as of 2026-09-02: 143 hardcoded font sizes
across 61 files" — and from then on report only what's new or what moved. The baseline number
trending down is its own useful signal, worth mentioning monthly.

## Output

```
**<rule name>** · <severity>
`<file>:<line>` — <what's there> → <what it should be>
<who and when, from git blame or Figma history>
<one line: why this one matters, if it isn't obvious>
```

Worked example:

> **no-hardcoded-font-size** · should-fix
> `src/components/InsightCard.tsx:44` — `fontSize="10px"` → there is no 10px step; the scale is
> sm=12 / md=14 / lg=16.
> Added by an agent-generated commit, Sep 1.
> Third 10px this month — the generator has a stale prompt somewhere, worth fixing upstream instead
> of one at a time.

That last line is the real value. One violation is a typo; three of the same violation is a broken
process, and the report should say which one it's looking at.

## Judgement

- **Report the violation, not the person.** Blame data is for finding the upstream cause, not for
  attribution. Frame around the artifact.
- **Deliberate exceptions exist.** A violation with a comment explaining why is not a violation.
  Respect it, and if the same exception appears three times, suggest the rule needs an escape hatch.
- **Don't fix unprompted.** Report. In an interactive run you can offer to fix; scheduled runs never
  touch code.
- **Near-misses are worth a mention when they cluster.** Something that isn't in the rules but keeps
  showing up is a candidate rule — propose it rather than silently enforcing it.

## Agent-built UI

Increasingly the largest source of drift, and the one this skill is most useful for. Generated code
tends to hardcode rather than import, because the generator often doesn't have the design system in
context.

When a violation traces to generated code, say so, and treat the fix as upstream: the prompt, the
skill file, or the context the generator gets. Fixing the output one component at a time is a
treadmill.

## Growing the ruleset

When the user states a rule in conversation — "we never use green for selected states" — offer to
add it. Ask the one question that makes it checkable: *how would I spot it?* If there's no answer,
it goes in `CLAUDE.md` instead, and say that's what you're doing.
