---
status: accepted
date: 2026-07-17
---

# Pair writing skills with audit companions

## Context and Problem Statement

Audits are the primary countermeasure for failures the writer cannot see (ADR 0001). Audit material can live inside the writing skill as a references/ file, or in a companion skill. Token savings do not decide this: agents given a writing task load both skills of a pair anyway. The question is what makes the audit reliably fire once the draft is done.

## Considered Options

* Pair each writing skill with an audit companion skill
* Keep audit material as references/ files inside the writing skill

## Decision Outcome

Chosen option: pair each writing skill with an audit companion, because a skill's description is the only layer that is always resident in context and survives compression. The companion's description ("useful after writing...") is a standing cue matching the moment the draft is done, a second trigger path for the audit. Co-housed material has only one path, an in-body instruction to revise after writing, which loses force under load (ADR 0001) and had already proven unreliable in practice. A companion also allows audit-only invocation and a clean handoff to a fresh-context auditor.

The writing skill keeps the norms and defines the target state; the companion holds the auditor's stance, procedure, and failure examples, with example collections in its references/ layer. Names follow the fal-write-X / fal-improve-X pattern; fal-japanese becomes fal-write-ja with companion fal-improve-ja. A pair exists only where auditing is a real event: fal-design stays unpaired; design failures are countered at decision time, and code review covers the rest.

### Consequences

* Good, because the audit has a standing, compression-proof cue and can run independently of the writing session.
* Good, because the collection has one legible shape.
* Neutral, because a pair typically loads together at write time; accepted, since the checklist is then at hand when writing ends and the example collections stay deferred.
* Bad, because a companion depends on its writing skill; installing one alone is incomplete.
* Bad, because the rename breaks existing installs and references to fal-japanese.
