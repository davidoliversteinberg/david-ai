# Working memory

The hot cache. Everything here loads into context every session, so it stays short — roughly 50–100
lines, 30ish people and terms. Anything longer or rarer lives in `memory/` and gets read on demand.

If a section grows past its budget, that's the signal to move the cold half into `memory/` and leave
a pointer. `consolidate-memory` does this on a schedule; you can also just ask.

## Who I am

- **Role:** <title>
- **What I'm accountable for:** <one line>
- **This quarter:** <one line — the thing that matters most>

## People

Name — role — how we intersect. Manager, skip-level, and the handful you actually work with.
Full profiles in `memory/people/`.

- **<Name>** — <role> — <how you intersect>

## Projects

Active only. One line each. Detail in `memory/projects/<slug>.md`.

- **<project>** — <status, and what you're waiting on>

## Shorthand

Terms and acronyms that would otherwise get misread. Company-specific ones especially.

- **<term>** — <what it means>

## Standing context

Things that are true across sessions and would change an answer if forgotten.

- <e.g. "Ships behind a flag until GA in November">

---

*Deep memory: `memory/glossary.md`, `memory/people/`, `memory/projects/`, `memory/context/`*
