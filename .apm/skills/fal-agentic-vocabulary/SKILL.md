---
name: fal-agentic-vocabulary
description: A shared vocabulary between the user and the agent for work performed by LLM agents. The vocabulary is made of named principles and antipatterns covering documentation, comments, and software design decisions. Useful for learning what the user cares about before substantial writing or design work. Useful for reading the intent behind feedback that names or implies one of these concerns. Useful in changes that look simple but affect structure, and where style or quality is not the stated concern. Counters characteristic failures of LLM-generated output.
---

# Agentic vocabulary

This skill names the concerns that recur in documentation, comments, and software design produced by LLM agents. It is a vocabulary rather than a rulebook. Reading it before substantial work shows what the user cares about. Whether a principle governs a particular case remains a judgment, and the vocabulary supplies the terms for making that judgment rather than the answer.

Five biases of LLM-generated output drive the failures named here. Each bias is a default that the vocabulary works against:

- **Anchoring bias.** The existing visible structure over-determines the output. Work stays on the immediate change, and the surrounding content goes unexamined.
- **Additive bias.** Generating costs nothing. Reading and maintaining cost everything. The writer adds material without the selection, search, reuse, and reduction that would come first.
- **Contextual bias.** The context that the writer works in, which the reader does not share, reaches the output anyway.
- **Compression bias.** The writer pursues brevity by condensing the expression rather than by selecting what to keep.
- **Defensive bias.** The writer avoids breaking, removing, dropping, and admitting error in favor of adding, keeping, accepting without reporting, and defending in advance.

Each principle has a section of its own. The section states the principle and what it asks of the writer, together with a test where the principle has one. Each antipattern is defined in the section of the principle that primarily prevents it, and other sections refer to it by name.

The vocabulary is in force throughout the work, whether or not anything invokes it.

A difficulty met in the work is one occasion when the vocabulary surfaces. What resists local adjustment is usually a sign of one of the failures named in the sections that follow. Examples include a name that remains unsatisfactory, content that does not fit the structure, and an urge to hedge or to keep. The response is to consult the definition rather than to force the adjustment. A critique that questions one of the work's premises counts as the same kind of signal. So does a critique that questions the criterion on which one of the work's classifications rests.

Feedback from the user is another occasion when the vocabulary surfaces. A remark can name a concern directly, or describe only its symptom. For example, a complaint about a name is usually about the structure that the name is asked to express, a case that the Structural naming principle governs. Any concern in the vocabulary can arrive this way.

## What the unit contains

### Principle of Necessity

Necessity asks that anything added earn its place, weighed against two costs: the reader's attention now and the project's maintenance later. The default is to leave it out. What earns a place is what the reader needs and cannot get from what is already there, not merely what has not been said yet. In documentation, that is most often a contract or a non-obvious why.

The test is removal: take the content out, and name what the reader then loses.

Brevity comes from selection, not compression. What does not earn its place is dropped, not condensed.

Being asked to make something clearer is not a license to add. When the point is already on the page, an addition more often buries it than sharpens it, and the first answer is rewriting what is there.

The **"over-documentation"** antipattern is content that does not earn its place. Two of its forms have names of their own.

The **"redundant statement"** antipattern restates what the artifact already says, and the copy becomes inaccurate as the original changes.

The **"unsolicited justification"** antipattern defends a claim that needs no defense, most often against an objection imagined while writing. The preemptive defense introduces the misreading that it tries to prevent.

The **"staleness surface"** antipattern is a statement that a routine change can silently falsify. Where the same point can be made one abstraction level up, that form remains accurate after the change.

The **"overlong block"** antipattern is a paragraph, list item, function, or code block that runs longer than its role needs. Length is what shows, and the cause lies elsewhere: content that does not earn its place, a focus that has split, or detail whose home is a smaller scope. The content question comes first, because splitting content that should have been dropped is wasted work.

The principle can be overapplied. A statement that carries what the reader genuinely lacks earns its place, however short the text would be without it. A qualifier that would make the statement false if removed is necessary. This practice is calibration, not minimization.

### Principle of Unit focus

Unit focus asks that a unit carry one role, at whatever level the unit sits: a sentence, a paragraph, a list, a function, a type. Units come to carry more than one role because the structure chosen while the unit was small stays fixed as content accumulates inside it. The principle asks for a composition of focused parts, not a larger unit.

The test is naming: can the unit's role be stated in one phrase, without an "and" joining two roles?

Notation follows from the role. A sentence carrying two thoughts becomes two sentences. A few short items stay inline in a sentence, and list notation begins where the inline form stops being readable. A list whose items are not genuinely parallel is rebuilt from the question of what is being enumerated, or converted back to prose. In code, orchestration and low-level manipulation sharing one body obscure each other.

A split need not follow the unit's current seams. Where connected roles resist a cut in place, the surrounding region is recomposed from its roles, and the unit's boundaries move with it.

The **"split focus"** antipattern is a unit carrying more than one role or concern. It appears at every structural level, and it is independent of length. A short unit can have split focus, and a unit with a single role can still run too long, a failure named the "overlong block" antipattern.

The principle can be overapplied by splitting what belongs together. Connected reasoning recast as bullet points drops the connections. A tiny helper extracted from a function forces readers to consult it to understand the parent's flow.

## What owns the unit

### Principle of Owner

Owner asks that placement, classification, and abstraction level be derived from the thing that governs a unit rather than from the current arrangement. The owner can be a boundary, a module, the party that guarantees a constraint, or a pattern shared across siblings. It need not be a tangible entity. When the owner is a shared pattern, that pattern is usually undocumented, and departing from it still breaks the consistency that it holds.

The test is to name what can make the unit wrong. Whatever can make it wrong owns it, and the unit belongs in that owner's scope. If the owner cannot be determined, then the assignment is a decision that belongs to the user.

"Home," the place where a unit belongs, follows from the owner. A home fixes an abstraction level as well as a location. A module header states its responsibilities, a function doc states its contract, and a type doc states its role. Orientation material belongs where a newcomer looks first.

In documentation, a document set split into overview and detail, such as a skill file and its references or a README and `docs/`, has already assigned homes. Worked examples and per-item explanations belong to the detail layer.

The same derivation runs in code. Logic belongs where its subject lives, and a caller that has to access another module's internals indicates a boundary drawn in the wrong place. When responsibilities share a location, the question is whether they share an owner or only a place: an accidental neighbor reads as a deliberate one.

A constraint belongs to whoever guarantees it. A rule that the module itself guarantees belongs in its own types. By contrast, a rule that only one use site imposes belongs in that use site's layer, stated with that owner as its subject.

Where structure and content conflict, the Owner principle decides which of the two is adjusted. If the sibling set owns the structure, then the unit conforms, a case that the Sibling consistency principle governs. If the content owns it, then the structure is revised, a case that the Structural fix principle governs.

The **"wrong home"** antipattern is a unit placed outside the scope that owns it. The displacement can be horizontal or vertical. A horizontal displacement is a wrong location, and a vertical displacement is a wrong abstraction level. The urge to repeat a caveat "to be safe" is usually this antipattern: the caveat sits outside its home, or two scopes overlap.

The **"snapshot reasoning"** antipattern reads the current state of usage as a design property. That state includes who calls a thing, what references it, and which inputs occur. The property is only a snapshot, and the next change might invalidate it.

### Principle of Structural fix

Structural fix asks that a difficulty be traced to its cause before it is addressed. Difficulty in writing, naming, or fitting content into a structure is a symptom, and the cause usually sits one level of structure above. An awkward API indicates a problem in the model behind it rather than in the surface.

The test is to ask what would have to be different one level up for the difficulty to disappear. A concrete answer is a lead rather than a verdict, because some change one level up can always absorb a difficulty. What marks the cause is that the difficulty dissolves rather than moves.

The **"wrong-layer patch"** antipattern works at the symptom layer and leaves the structural cause untouched. Each of the following substitutes for the structural fix: padding an empty section so that every heading has a paragraph, adding a run-time check for a state that the types could have excluded, and writing prose to compensate for a structural awkwardness.

### Principle of Redesign

Redesign asks that a structure be rebuilt to be coherent under changed assumptions rather than patched under the old ones. It specializes the Structural fix principle. The difference is the trigger: the Structural fix principle starts from a visible symptom, and the Redesign principle starts from a premise that has changed. The result reads as if it had always been this way.

The test is whether a writer who never saw the previous version would draw these same boundaries, sections, and types.

A redesign can name what it makes unnecessary: the functions that it replaces, the helpers that it generalizes, and the special cases that it absorbs. Every existing element needs that decision, and having read the existing code is not the same as having decided what it should become.

During the work, the signal is the urge to make the content fit the structure that is already there. The urge means that the assumptions have already shifted, and the shift calls for rederiving the affected scope from its responsibilities.

The **"under-scoped change"** antipattern lets the old structure, rather than the changed assumptions, decide how far the editing reaches. It specializes the "wrong-layer patch" antipattern: here the symptom layer is the spot already under edit. Its most frequent form is a forced insertion: adding a flag, wrapping with a condition, or inserting a special case.

The **"vestige"** antipattern is a remnant, such as dead code, superseded structure, or an assumption no longer in force, that the current design has made unnecessary. It is an under-scoped change seen from its result: some edit omitted the deletions that the design required, and that edit need not be the change in hand. "Vestigial" means superseded, not merely unexercised: code that nothing reaches today still belongs when the design gives it a reason to exist.

When the change called for replacement, the **"needless backward compatibility"** antipattern keeps the old interface alongside the new one. It is an under-scoped change made deliberately: where a vestige remains because it was overlooked, here the old interface is kept on purpose. The trigger is the writer's own thought that the callers must not break, substituting for a decision that was not made. Whether the old interface survives belongs to the user, and a request for the change that says nothing about keeping it has already answered.

## What is already there

### Principle of Sibling consistency

Sibling consistency asks that a unit match the form that its siblings already share: density, tone, vocabulary, language, structure, naming convention, argument order, and a module's layout. That shared form is information that readers carry from one sibling to the next. A locally better choice in one of them costs the whole set.

The test is to state the form that the siblings share before writing the unit. A unit written without that statement conforms only by accident.

Where the shared structure allows variation, such as optional sections or permitted merging, using that flexibility is conforming rather than padding. Where the material does not fit even then, the mismatch is reported rather than resolved silently in one place, a case that the Visible conflict principle governs.

Consistency also decays through editing. A change made in one place while its linked copies and callers go unvisited leaves the set consistent today and mismatched later.

The **"sibling mismatch"** antipattern is a unit whose form departs from its siblings. The usual forms are a list item several times the length of those around it, a passage written in a different language from the document, and a module laid out unlike its neighbors. The nearest sibling set is the document itself, and the terms that it has already used are part of the form that it shares. A second term for a concept that the document has already named reads as a second concept.

The principle can be overapplied. When a unit is adjusted to match sparser siblings, the calibration comes from removing what does not earn its place, a case that the Necessity principle governs. It does not come from condensing the remaining text.

### Principle of Discovery

Discovery asks that what already exists be found first, and that the addition be related to it. The search extends beyond the unit being changed. This is because an existing type for the same concept usually lives in another module. An existing statement of the same fact usually lives in another document.

The test is to name what the addition relates to: the element that it extends, specializes, replaces, or deliberately sits beside. When the addition can name none of these, the writer has usually not searched.

What the search protects is the relation. Without that relation, improvements to the existing element never propagate to the new one, and readers are left to determine for themselves how the two relate.

Judging an existing element and finding it lacking is a decision. Not knowing that it exists is not a decision.

The **"unintegrated addition"** antipattern is a new element placed without being related to the existing model. It takes several forms: something that overlaps what is already there, a specialization of an existing general type defined as though unrelated, and a section written in its own vocabulary and classification rather than the one already in use. The degree of overlap is not the point, and an exact duplicate is only the most extreme case.

### Principle of Visible conflict

Visible conflict asks that a mismatch between two things that must correspond be reported rather than hidden. In code, an invariant that the type system cannot enforce is asserted rather than hidden by a default, a suppressed error, or a substituted null. That is the established fail-fast principle. In documentation, when a description and the thing that it describes disagree, the conflict is reported rather than resolved by quietly moving one side to match the other.

The test is whether a violation would reach someone who can act on it. Absorbing it, whether by a default value or by an edit that makes a disagreement invisible, defeats the principle.

The **"silent fallback"** antipattern hides a violation of an invariant that should have held, such as returning a default, suppressing an error, or substituting null. It then proceeds as though nothing happened.

The **"silent contradiction"** antipattern is a disagreement between a description and the thing that it describes, one that remains undetected because neither side was checked against the other.

## What lies outside the boundary

### Principle of Reader's position

Reader's position asks that output start from what the reader can see rather than from what the writer knows. The first judgment is who the reader is and where they stand, because everything that follows is weighed from that position. The writer holds three things that the reader does not, and each enters the output in its own way:

- **Boundary.** The writer sees internals and current callers. The reader has only what the boundary exposes. The Contract principle governs this concern.
- **Session.** The writer holds instructions, discussion, and the previous version. The reader has none of it. The Session-blind principle governs this concern.
- **Namespace.** The writer knows the referent and can read a loose term back. The reader can follow only terms that resolve in a public namespace. The Words that resolve principle governs this concern.

A symbol name and a doc comment are addressed to the same reader, and the same three concerns govern both.

### Principle of Contract

Contract asks that a name or a description say what its boundary promises to the outside, and nothing besides. Placement decides which boundary applies. A public function's name and doc comment stand at the public boundary, a comment inside the body stands within it, and a design note inside a module stands at the module's edge. The Contract and Owner principles converge here: the owner fixes the home, and the home fixes the boundary.

The test is ownership: can the name or the description be restated as a promise with the thing itself as its subject? A mechanism enters the promise only by being fixed there: stated at the boundary, it stops being free to change, and so stops being internal. What is meant to stay replaceable cannot be promised. Facts about current callers are observable from outside but are not owned.

The vocabulary of the promise is bound the same way. This is because a promise stated in terms visible only from inside is not usable by the reader to whom it is addressed.

Names and descriptions addressed to internal readers are outside this principle. Where an implementation note belongs is a question for the Owner principle. Crossing the boundary outward has one use: explaining behavior that is observable from outside and otherwise surprises.

The **"exposed internals"** antipattern carries implementation concepts across the boundary to a reader who cannot see them and never asks about them.

The **"caller-bound framing"** antipattern names or describes a thing from its current caller's viewpoint rather than by what the thing promises. The two are opposite forms of the same boundary violation: one carries inward material out, and the other brings outward material in.

### Principle of Session-blind

Session-blind asks that the work read as though this session had never happened.

The test is whether a writer who reached the same understanding without this session would state this fact, use this framing, give this point this weight, and reach for this example.

The session is not only what is visible in the conversation. A line of reasoning that occurred only in the writer's own thinking counts too. A test limited to "was this discussed?" misses exactly that part.

A rejected alternative deserves particular suspicion, because from inside the session it always looks justifiable. The alternative was rejected for a reason, so recording it feels like documentation.

When the rejected alternative is what a fresh reader expects and the surprise of the choice needs addressing, mentioning the alternative has a rare legitimate use. The session biases that very judgment, though: the alternative feels expected mostly because it was recently discussed. What decides the case is whether a reader who has never heard of the alternative gains anything from being told about it.

The **"session leak"** antipattern is a trace of the session in the output: an instruction mixed in, a rejected alternative mentioned, and a pointer to something that exists only for this session.

The **"salience leak"** antipattern is the session increasing the emphasis that a point receives or narrowing the example chosen for it.

### Principle of Version-blind

Version-blind asks that the current state be described by a writer who never saw the previous one. It specializes the Session-blind principle to the part of the session that is the old version. The artifact's purpose decides whether the principle applies. A document that exists to record a decision or an event carries the history that it exists to record.

Two failures take opposite forms, and writing as though the old version had never existed prevents both.

The **"history leak"** antipattern writes as though the reader shares the old version. A qualifier that describes the current state relative to a previous one is not empty, which is what lets it survive: the change is still vivid to the writer, and even a fresh audit can rate the comparison informative. The failure is on the reader's side, because a reader who is not reading for history wants the current state alone.

The **"unsolicited history"** antipattern does the opposite. It narrates the change for a reader assumed not to know it: what replaced what, what a thing used to be called. It is typically well-intentioned. It also fails the Necessity principle, because the added history does not earn its place.

### Principle of Words that resolve

Words that resolve asks that every term and phrase reach a meaning that the reader already has access to. Three namespaces are available: the code symbols of the repository, the established terms of the domain, and the terms explicitly defined in the project's tracked documents. The requirement extends beyond single terms to phrases, where the reader must be able to recover the relationship between the words.

The test is resolution: can the reader reach the meaning of every term and phrase through one of the three namespaces?

Two practices keep terms resolvable. A concept that recurs often enough within one document to want a name resolves after it is defined at first use. However, a name intended to persist beyond the document is a design decision that belongs to the user. Conversely, an unfamiliar term already in the repository is worth checking for a definition site before it is adopted. This is because a term appearing only in passing prose is likely an earlier session's coinage that further use establishes as vocabulary.

In code, a concept that only a general-purpose word names is usually one whose contract or abstraction level is not yet defined. The Structural naming principle routes that case to the Contract and Structural fix principles.

The **"unmarked coinage"** antipattern is a term that the reader cannot follow to any referent. It usually arrives in one of two ways: a private label from the session enters the output, or condensing an expression produces a compound that the writer can understand only because they already know the meaning. Typography can strengthen the false claim, because backticks suggest a code symbol and capitalization suggests a defined term. However, the failure lies in the term rather than the markup.

The **"catch-all name"** antipattern makes a general-purpose word the fixed name of one specific concept. The name then resolves only to the word's broad everyday sense, and the reader cannot distinguish it from its neighbors. Using such a word in its plain generic sense, where context fixes the referent, is not this failure.

The **"stripped term"** antipattern drops the qualifier from a defined term at a later mention, leaving a bare word that resolves only to its everyday sense. The full term belongs wherever it names its concept, and shortening is safe only where context has already fixed the referent.

The **"packed phrase"** antipattern condenses an expression until the reader can no longer recover how its words relate. Each word can resolve on its own, and what the condensation drops is the relation between them.

### Principle of Structural naming

Structural naming asks that a name be treated as a claim about the structure rather than as a label to swap. A name declares what a concept presents to its scope, and it also has to distinguish that concept from the ones beside it. A bad name is a symptom of one of three causes, worth suspecting in this order:

1. The concept is not sufficiently defined, a case that the Structural fix principle governs. The name is being asked to carry a distinction that the design does not express. Restructuring the region sometimes removes the thing that needed naming.
2. The name does not state what the thing promises, a case that the Contract principle governs. In its place stands either the caller's viewpoint, named the "caller-bound framing" antipattern, or an internal fact, named the "exposed internals" antipattern. A third possibility is what the thing is in itself. Such a name is true, and it still does not fix the role that the thing plays for its scope.
3. The name does not identify its referent, a case that the Words that resolve principle governs. It is either too broad, named the "catch-all name" antipattern, or opaque, named the "unmarked coinage" antipattern.

Which cause is most common depends on the medium. In code, causes 1 and 2 come first, and a simple rename rarely suffices. In documentation, the surface fix at cause 3 is often enough.

Using a name consistently after it is chosen is a separate concern. The "stripped term" antipattern belongs to the Words that resolve principle rather than here.

## Companion skills

Japanese has failure modes beyond what the preceding sections cover. The `fal-write-ja` skill owns these failure modes. Whenever the output is Japanese, that skill applies together with this one.

Having applied these concepts while writing does not make checking the finished work redundant. Believing otherwise is itself a self-evaluation from the writer's position, which the Reader's position principle warns about. The `fal-agentic-audit` skill supplies that separate process: check-by-check cues for how each failure shows in a finished change, and the discipline for fixing what the checks find.
