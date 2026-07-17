---
status: accepted
date: 2026-07-17
---

# Keep scanning material whole

## Context and Problem Statement

ADR 0001 settles when content loads, not how finely a body or a deferred file may be split. Skill authoring guidance generally favors progressive loading (split a skill into small parts and read only what a task calls for), and measured against it these skills look coarse: whole SKILL.md bodies loaded at once, an example collection read in full every audit. So the question recurs across the collection, not for any one skill: should fal-write-docs, fal-design, and the rest be cut into finer parts loaded on demand?

## Considered Options

* Keep scanning material whole and split only what is looked up by name
* Split further by topic and load the parts a task calls for

## Decision Outcome

Chosen option: "Keep scanning material whole and split only what is looked up by name", because progressive loading assumes reference material retrieved by name, and most of what these skills carry acts whole instead.

A generation-time body must be resident before the work it guides. A scanning collection must be read whole because finding which parts apply is the audit itself: the failures look natural to whoever wrote them (ADR 0001), so an auditor who could tell up front which parts to read would not need the collection. The stop signals in fal-design §1 are no counterexample, since they surface in the work situation, not inside the artifact under review.

The criterion, once ADR 0001 has fixed a piece of content and its acting moment: does all of it act, or only a part addressable by name?

* Material looked up by name (dictionaries, references, per-topic procedures) may be split.
* Material that acts whole (a resident body, a collection scanned against an artifact) stays whole. Splitting does not defer the unread parts. It drops them from the work.

### Consequences

* Good, because the recurring proposal to split further now has a stated criterion to meet, instead of being reargued each time.
* Good, because an audit covers every failure type its collection records, not the ones already suspected.
* Bad, because collections have a size ceiling and need pruning to stay under it.
