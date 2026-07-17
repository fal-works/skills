---
status: accepted
date: 2026-07-17
---

# Assign skill content to the moment it acts

## Context and Problem Statement

The skills load substantial instruction text into everyday sessions, much of it countering failures that are the model's default behavior. The same structural questions kept returning: split or slim the always-loaded bodies, or turn the skills into post-hoc audit checklists? In practice, principles read at session start often failed to act at the moment of decision, while audits against explicit criteria caught what generation had missed.

## Considered Options

* Assign content to the moment it acts
* Keep each skill self-contained in one always-loaded SKILL.md
* Specialize all skills for post-hoc audit and leave generation unguided

## Decision Outcome

Chosen option: "Assign content to the moment it acts", because instructions influence generation only weakly and lose force under load, recognition against explicit criteria is far more reliable than compliance during generation, and the cost of a late fix differs by failure type. Timing, not optionality, is the splitting criterion.

* Generation-time defaults, plain conventions, and in-flight signals (the stop signals in fal-design §1) stay in the always-loaded SKILL.md bodies; they matter most where a late fix is expensive.
* Audit material (how violations look, fixes, procedures, failure examples) loads only at audit time, preferably in a fresh context. Failure examples stay out of the bodies: they promote the very patterns they describe, and compression can strip the negation.
* Recurring failure types carry stable names and rule-stating headings, so the skeleton survives context compression.

### Consequences

* Good, because everyday sessions carry only the generation-time defaults.
* Good, because failures invisible to the writer get a mechanism that actually catches them.
* Bad, because substantial prose work becomes two passes; audits are therefore scoped to artifacts that persist.
* Bad, because bodies and audit companions cross-reference and must stay coherent.
