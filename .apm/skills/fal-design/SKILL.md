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

When a requirement changes the assumptions under existing code, redesign the affected structure to be coherent under the new assumptions. The target is a design that reads as if it had always been this way: no vestigial shapes from the old approach, no names that only make sense in light of what was there before.

The characteristic failure is the minimal-diff fix: adding a flag, wrapping with a condition, inserting a special case. Each preserves the old structure at the cost of coherence. A parameter added "just for this case" signals that the abstraction boundary is in the wrong place.

A redesign can name what it makes unnecessary: the functions it replaces, the helpers it generalizes, the special cases it absorbs. Treat the change as one design across old and new elements, deciding which stay, which generalize, and which go, rather than as a new API beside the existing one. When a proposal leaves every existing element in place, suspect that the decision was never made for them; having read the existing code is not the same as having decided what it should become.

Certain signals mean the assumptions have already shifted and local adjustment should stop:

- Entry points multiplying per use site.
- A new function sharing an existing function's responsibility.
- A new type enumerating nearly the same cases as an existing one.
- A use-site-specific constraint aimed at a general module.
- A name that will not settle, whatever vocabulary is tried (§4).

At any of these, or when a critique casts doubt on the classification axis or a premise, do not refine the current proposal; rederive the affected scope from its responsibilities.

Self-check: if I had never seen the old design, would I draw the same boundaries and choose the same types?

## 2. Responsibility and boundaries

Every module, type, and function owns one clear responsibility. Derive placement from what a concept *is*, not from who currently uses it.

- The current state of the code (who calls a thing, what references it, which inputs occur) is a snapshot, not a design property. That module A is today referenced only by module B does not make A a part of B; a future module C may reference it with equal right. Every decision read off the snapshot deserves the same suspicion: classification, placement, and whether code should exist all follow from contracts and invariants.
- Logic belongs where its subject lives. If a caller must reach into another module's internals, the boundary is drawn wrong.
- When multiple responsibilities share a location, ask whether they share it for a reason (cohesion) or by accident (convenience). Accidental neighbors become real coupling over time.

## 3. Domain modeling

Model the domain before writing control flow. The data types should make illegal states unrepresentable; the implementation then follows from what the types permit.

- The domain is usually already modeled in part. Before designing a new type, search for an existing type that represents the same concept, beyond the module being changed. Evaluating an existing type and finding it lacking is a design judgment; not knowing it existed is not, even when the resulting definitions look alike.
- Prefer a tight model that eliminates invalid combinations over a loose model that checks validity at runtime. An optional field that "should never be null when X is true" is a design gap, not a defensive measure.
- A constraint has an owner. Before making a state unrepresentable, determine whose contract forbids it: a rule the module itself guarantees belongs in its types; a rule that only one use site imposes belongs in that use site's layer, stated with that owner as its subject.
- A state is impossible when a contract or an invariant excludes it, so that no path reaches it without changing the design itself. That nothing reaches a state today (no caller, no dependent module, no input that selects it) is not impossibility; it is ordinary headroom in a general mechanism, and needs no guard.
- When full enforcement of an impossible state is impractical, keep it explicit as an assertion, not a silent fallback.
- The model shapes the API. If the API is awkward, suspect the model before suspecting the surface.

## 4. Naming

A name declares what a concept is. It should resolve to the concept's own responsibility, not to one caller's view of it. Getting that right is necessary and not sufficient: a name also has to hold its concept apart from the ones beside it, which makes naming structural work rather than vocabulary work.

- Name by what the thing does or represents, at its own level of generality. A utility named by its first consumer's domain becomes incoherent when other consumers arrive.
- When a name will not settle, the difficulty is evidence about the structure. Similar names crowding a region, a qualifier added only to keep two names apart, a name that resolves only through its use site: each reports a distinction the design does not carry. Restructure the region rather than searching for finer names; the restructure sometimes removes the thing that needed naming.
- A name coined during implementation is a design claim. Verify that it refers to a real concept in the domain, not a session-local label for "the thing I'm building right now."

## 5. Abstraction and cohesion

A function should read as one clear thought at one level of abstraction.

- Prefer one abstraction level per function. High-level orchestration and low-level manipulation in the same body obscure both.
- Do not extract a helper merely because a block is long. Extract when the block has its own responsibility and a name that clarifies the parent. A tiny helper that readers must chase to understand the parent's flow is a net loss.
- Remove code the current design has left vestigial. Vestigial code is not insurance; it is noise that future readers will try to understand. Vestigial means superseded by the current design, not merely unexercised: code that nothing reaches today still belongs when the design gives it a reason to exist and ordinary extension will bring its users.
