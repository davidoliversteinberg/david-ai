# System rules

The design-system rules `system-watchtower` checks for. Only put rules here that are
**mechanically checkable** — a rule you can't detect in a diff or a Figma file is a principle, not
a rule, and it belongs in `CLAUDE.md` instead.

Start with four or five. A watchtower that reports twenty violations a week gets muted; one that
reports two real ones a month gets read.

## Format

Each rule needs: what's forbidden, what to do instead, how to detect it, and where it applies.
The **detect** line is what makes the rule usable — without it the scan is a vibe check.

---

### <rule-slug>

- **Rule:** <what must or must not be true>
- **Instead:** <the correct alternative>
- **Detect:** <a grep, a token name, a prop pattern, a Figma layer property>
- **Applies to:** <repos, Figma files, or "everywhere">
- **Severity:** blocking | should-fix | note
- **Why:** <one line — the reason, so a near-miss can be judged on intent>

---

## Rules

<!-- Add yours below. Example shape: -->

### no-hardcoded-font-size

- **Rule:** No raw pixel font sizes in component code.
- **Instead:** Use the named scale tokens.
- **Detect:** `fontSize="` or `font-size:` followed by a number+px in `.tsx`/`.css`.
- **Applies to:** all product repos
- **Severity:** should-fix
- **Why:** raw sizes bypass the scale, so they don't respond to density or theme changes.

## Scope

`system-watchtower` scans only what it can actually reach:

- `~~code` — diffs and PRs in the repos listed under **Applies to**
- `~~design` — recently edited Figma files, if a design connector exists

It never guesses at a codebase it can't read. If a rule's **Applies to** names a repo the watchtower
can't reach, it says so once and skips the rule rather than reporting a false all-clear.
