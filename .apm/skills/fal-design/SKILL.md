---
name: fal-design
description: Principles for software design decisions in any general-purpose programming language. Useful when a task involves structural decisions about code, such as planning an approach, adding capabilities, restructuring, or modifying abstractions. Also useful in changes that look simple but affect structure. Counters the LLM tendency to patch minimally rather than redesign coherently.
---

# Design

Software design here means any structural decision about code: what concepts exist, what each one owns, where boundaries fall, how data is modeled, and how control flows.

LLM-generated designs fail in predictable ways, driven by one dominant tendency: **anchoring to what exists.** The model sees the current code and extrapolates from its structure, even when the requirement calls for a different structure. This produces:

- Patches that graft new behavior onto old scaffolding instead of reshaping the scaffolding.
- Naming and placement driven by the current caller rather than by the concept itself.
- Data models that tolerate states the domain forbids, because tightening them would mean changing more.
- Mixed abstraction levels within a single unit, because the existing code already mixed them.

The sections below counter this tendency. They are not steps in a procedure; they are lenses to apply concurrently when making or evaluating a design decision.

## 1. Redesign over patching

When a requirement changes the assumptions under existing code, redesign the affected structure to be coherent under the new assumptions. The target is a design that reads as if it had always been this way — no vestigial shapes from the old approach, no names that only make sense in light of what was there before.

The characteristic failure is the minimal-diff fix: adding a flag, wrapping with a condition, inserting a special case. Each preserves the old structure at the cost of coherence. A parameter added "just for this case" signals that the abstraction boundary is in the wrong place.

Self-check: if I had never seen the old design, would I draw the same boundaries and choose the same types?

## 2. Responsibility and boundaries

Every module, type, and function owns one clear responsibility. Derive placement from what a concept *is*, not from who currently uses it.

- A module's classification follows its responsibility, not its current reference graph. That module A is today referenced only by module B does not make A a part of B; a future module C may reference it with equal right.
- Logic belongs where its subject lives. If a caller must reach into another module's internals, the boundary is drawn wrong.
- When multiple responsibilities share a location, ask whether they share it for a reason (cohesion) or by accident (convenience). Accidental neighbors become real coupling over time.

## 3. Domain modeling

Model the domain before writing control flow. The data types should make illegal states unrepresentable; the implementation then follows from what the types permit.

- Prefer a tight model that eliminates invalid combinations over a loose model that checks validity at runtime. An optional field that "should never be null when X is true" is a design gap, not a defensive measure.
- When full enforcement is impractical, keep the impossible state explicit as an assertion, not a silent fallback.
- The model shapes the API. If the API is awkward, suspect the model before suspecting the surface.

## 4. Naming

A name declares what a concept is. It should resolve to the concept's own responsibility, not to one caller's view of it.

- Name by what the thing does or represents, at its own level of generality. A utility named by its first consumer's domain becomes incoherent when other consumers arrive.
- When many names in a region start to look similar, the region likely needs restructuring, not finer naming distinctions.
- A name coined during implementation is a design claim. Verify that it refers to a real concept in the domain, not a session-local label for "the thing I'm building right now."

## 5. Abstraction and cohesion

A function should read as one clear thought at one level of abstraction.

- Prefer one abstraction level per function. High-level orchestration and low-level manipulation in the same body obscure both.
- Do not extract a helper merely because a block is long. Extract when the block has its own responsibility and a name that clarifies the parent. A tiny helper that readers must chase to understand the parent's flow is a net loss.
- When a design change makes existing helpers or layers vestigial, remove them. An unused abstraction is not insurance; it is noise that future readers will try to understand.
