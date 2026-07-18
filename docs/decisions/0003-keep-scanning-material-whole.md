---
status: accepted
date: 2026-07-17
---

# Keep scanning material whole

## Context and Problem Statement

ADR 0001 settles when content loads, not how finely a body or a deferred file may be split. Skill authoring guidance generally favors progressive loading: keep the common workflow in SKILL.md and defer details whose relevance the task can identify. Measured against it, these skills look coarse: whole SKILL.md bodies loaded at once, an example collection read in full every audit. So the question recurs across the collection, not for any one skill: should fal-write-docs, fal-design, and the rest be cut into finer parts loaded on demand?

## Considered Options

* Keep scanning material whole and split only what can be selected before reading
* Split further by topic and load the parts a task calls for

## Decision Outcome

Chosen option: "Keep scanning material whole and split only what can be selected before reading", because progressive loading saves context only when information already available to the agent identifies which part is relevant. Most of what these skills carry acts whole instead.

Content that guides every instance of a phase must be resident before that phase. A scanning collection must be read whole because finding which parts apply is the audit itself: the failures look natural to whoever wrote them (ADR 0001), so an auditor who could tell up front which parts to read would not need the collection. The stop signals in fal-design §1 are no counterexample, since they surface in the work situation, not inside the artifact under review.

The criterion, once ADR 0001 has fixed a piece of content and its acting moment: can the agent decide that a part is irrelevant from information available before reading that part?

* Material selected by a known variant, artifact kind, workflow phase, or lookup key may be split.
* Material whose relevance can be determined only by applying it to the work stays whole. A split that every use must traverse does not defer content; it adds navigation and a chance to omit part of the work.

### Consequences

* Good, because the recurring proposal to split further now has a stated criterion to meet, instead of being reargued each time.
* Good, because an audit covers every failure type its collection records, not the ones already suspected.
* Good, because progressive loading remains available where the task identifies a relevant subset before reading.
* Bad, because collections that act whole have a size ceiling and need pruning or a different representation to stay under it.
