---
name: fal-agentic-vocabulary
description: A shared vocabulary between the user and the agent for work performed by LLM agents. The vocabulary is made of named principles and antipatterns that cover the artifacts of the work, such as documentation, comments, software design decisions, conceptual models, and plans. Useful for learning what the user cares about before substantial writing or design work. Useful for reading the intent behind feedback that names or implies one of these concerns. Useful in changes that look simple but affect structure, and where style or quality is not the stated concern. Counters characteristic failures of LLM-generated output.
---

# Agentic vocabulary

This skill names the concerns that recur in the artifacts that LLM agents produce. Documentation, code comments, and software design decisions are such artifacts. So are the more abstract structures that the work rests on, such as a conceptual model, a classification, or a plan. The vocabulary supplies terms for judging whether a principle governs a particular case, rather than rules that decide it.

Six biases of LLM-generated output drive the failures named here. Each bias is a default that the vocabulary works against:

- **Anchoring bias.** The existing visible structure and state over-determine the output. They are taken as given, and the output is fitted to them.
- **Local bias.** The writer examines only the unit under edit. The writer does not consult anything outside that unit, such as its siblings, the structure above it, and the thing that it must correspond to.
- **Additive bias.** Generating costs nothing. Reading and maintaining cost everything. The writer adds material without the selection, reuse, and reduction that would come first.
- **Contextual bias.** The writer writes from the working context without rebuilding the content for a reader who does not share that context. What the context made prominent is written even where the reader has no use for it. What the context made obvious is treated as known to the reader.
- **Compression bias.** The writer pursues brevity by condensing the expression rather than by selecting what to keep.
- **Defensive bias.** The writer avoids breaking, removing, dropping, and admitting error in favor of adding, keeping, accepting without reporting, and defending in advance.

Each principle has a section of its own. The section states the principle and what it asks of the writer, together with a test where the principle has one. Each antipattern is defined in the section of the principle that primarily prevents it, and other sections refer to it by name.

The vocabulary is in force throughout the work, whether or not anything invokes it.

A difficulty met in the work is one occasion when the vocabulary surfaces. What resists local adjustment is usually a sign of one of the failures named in the sections that follow. Examples include a name that remains unsatisfactory, content that does not fit the structure, and an urge to hedge or to keep. The response is to consult the definition rather than to force the adjustment. A critique that questions one of the work's premises counts as the same kind of signal.

Feedback from the user is another occasion when the vocabulary surfaces. A remark can name a concern directly, or describe only its symptom. For example, a complaint about a name is usually about the structure that the name is asked to express, a case that the Structural naming principle governs. Any concern in the vocabulary can arrive this way.

Under the Applicability principle, an antipattern name applies when the defined failure occurs, not merely when the case resembles a form mentioned in the definition. A failure that no definition describes is reported in plain words, and whether it receives a name is a decision that belongs to the user.

## What the unit contains

### Principle of Necessity

Necessity asks that anything added earn its place, weighed against two costs: the reader's attention now and the project's maintenance later. The default is to leave it out. What earns a place is what the reader needs and cannot get from what is already there, not merely what has not been said yet. In documentation, that is most often a contract or a non-obvious why.

The test is removal: take the content out, and name what the reader then loses.

Brevity comes from dropping what does not earn its place, not condensing it.

Being asked to make something clearer is not a license to add. When the point is already on the page, an addition more often buries it than sharpens it, and the first answer is rewriting what is there.

The **"over-documentation"** antipattern is content that does not earn its place. Two of its forms have names of their own: the "redundant statement" antipattern and the "unsolicited justification" antipattern.

The **"redundant statement"** antipattern restates what the artifact already says, and the copy becomes inaccurate as the original changes. Two passages are copies when either one would answer the same question for the same reader. Sharing a subject does not make two passages copies.

The **"unsolicited justification"** antipattern defends a claim that needs no defense, most often against an objection imagined while writing. The preemptive defense introduces the misreading that it tries to prevent.

The **"staleness surface"** antipattern is a statement that a routine change can silently falsify. Where the same point can be made one abstraction level up, that form remains accurate after the change.

The **"overlong block"** antipattern is a paragraph, list item, function, or code block that runs longer than its role needs. Length is what shows, and the cause lies elsewhere: content that does not earn its place, a focus that has split, or detail whose home is a smaller scope. The content question comes first, because splitting content that should have been dropped is wasted work.

Applying the principle too little is the more frequent failure. The principle can also be overapplied. Applying it is calibration, not minimization. Overapplication removes content that earns its place, such as the following:

- A statement that carries what the reader genuinely lacks, however short the text would be without it
- A qualifier that would make the statement false if removed
- A sentence that adds no new fact but prevents the "unstated relation" antipattern by stating a relation or a status where the reader needs it

### Principle of Unit focus

Unit focus asks that a unit carry one role, at whatever level the unit sits: a sentence, a paragraph, a list, a function, a type. Units come to carry more than one role because the structure chosen while the unit was small stays fixed as content accumulates inside it.

The test is naming: can the unit's role be stated in one phrase, without an "and" joining two roles?

Notation follows from the role. A sentence carrying two thoughts becomes two sentences. A few short items stay inline in a sentence, and list notation begins where the inline form stops being readable. A list whose items are not genuinely parallel is rebuilt from the question of what is being enumerated, or converted back to prose. In code, orchestration and low-level manipulation sharing one body obscure each other.

A split need not follow the unit's current seams. Where connected roles resist a cut in place, the surrounding region is recomposed from its roles, and the unit's boundaries move with it.

The **"split focus"** antipattern is a unit carrying more than one role or concern. It is independent of length. A unit with a single role can still run too long, a failure named the "overlong block" antipattern.

The principle can be overapplied by splitting what belongs together. A connective that states how its clauses relate is part of the content, not a seam. Where such a sentence is split for length, the split is complete only when the resulting sentences still state the relation. Connected reasoning recast as bullet points drops the relations in the same way. A tiny helper extracted from a function forces readers to consult it to understand the parent's flow.

## What determines the unit

### Principle of Placement

Placement asks that a unit sit in the place that covers its subject, at the abstraction level of that place. A unit is about something, and a place covers something. For example, a comment covers the declaration that it is attached to, a section covers its topic, and a module covers its responsibility. A unit outside the place that covers its subject fails in two ways. The change that falsifies the unit happens elsewhere, so nothing prompts its update. The reader looks for the unit elsewhere, or takes it as a property of the place where it sits.

The test is to name what the unit is about, and then to name what the place covers. The unit belongs where the two match.

The place where a unit belongs is its "home." A home also fixes an abstraction level. A module header states its responsibilities, a function doc states its contract, and a type doc states its role. Detail about a smaller thing belongs in the smaller scope, next to that thing. Orientation material belongs where a newcomer looks first.

In documentation, a document set split into overview and detail, such as a skill file and its references or a README and `docs/`, has already assigned homes. Worked examples and per-item explanations belong to the detail layer.

The same matching applies in code. Logic belongs where its subject lives, and a caller that has to access another module's internals indicates a boundary drawn in the wrong place. When responsibilities share a location, the question is whether they share a subject or only a place: an accidental neighbor reads as a deliberate one.

The subject of a constraint is whoever guarantees it. A rule that the module itself guarantees belongs in its own types. By contrast, a rule that only one use site imposes belongs in that use site's layer, stated with that use site as its subject.

The **"wrong home"** antipattern is a unit placed where its subject is not covered. This includes both a wrong location and a wrong abstraction level. The urge to repeat a caveat "to be safe" is usually this antipattern: the caveat sits outside its home, or two scopes overlap.

### Principle of Structural fix

Structural fix asks that a difficulty be traced to its cause before it is addressed. Difficulty in writing, naming, or fitting content into a structure is a symptom, and the cause usually sits one level of structure above. An awkward API indicates a problem in the model behind it rather than in the surface.

The test is to ask what would have to be different one level up for the difficulty to disappear. A concrete answer is a lead rather than a verdict.

The **"wrong-layer patch"** antipattern works at the symptom layer and leaves the structural cause untouched. Each of the following substitutes for the structural fix: padding an empty section so that every heading has a paragraph, adding a run-time check for a state that the types could have excluded, and writing prose to compensate for a structural awkwardness.

### Principle of Redesign

Redesign asks that a structure be rebuilt to be coherent under changed assumptions rather than patched under the old ones. It specializes the Structural fix principle. The difference is the trigger: the Structural fix principle starts from a visible symptom, and the Redesign principle starts from a premise that has changed. The result reads as if it had always been this way.

The test is whether a writer who never saw the previous version would draw these same boundaries, sections, and types.

A redesign can name what it makes unnecessary: the functions that it replaces, the helpers that it generalizes, and the special cases that it absorbs. Every existing element needs that decision, and having read the existing code is not the same as having decided what it should become.

During the work, the signal is the urge to make the content fit the structure that is already there. The urge means that the assumptions have already shifted, and the shift calls for rederiving the affected scope from its responsibilities.

The **"under-scoped change"** antipattern lets the old structure, rather than the changed assumptions, decide how far the editing reaches. It specializes the "wrong-layer patch" antipattern: here the symptom layer is the spot already under edit. Its most frequent form is a forced insertion: adding a flag, wrapping with a condition, or inserting a special case.

The **"vestige"** antipattern is a remnant, such as dead code, superseded structure, or an assumption no longer in force, that the current design has made unnecessary. It is an under-scoped change seen from its result: some edit omitted the deletions that the design required, and that edit need not be the change in hand. "Vestigial" means superseded, not merely unexercised: code that nothing reaches today still belongs when the design gives it a reason to exist.

When the change called for replacement, the **"needless backward compatibility"** antipattern keeps the old interface alongside the new one. It is an under-scoped change made deliberately: where a vestige remains because it was overlooked, here the old interface is kept on purpose. The trigger is the writer's own thought that the callers must not break, substituting for a decision that was not made. Whether the old interface survives belongs to the user, and a request for the change that says nothing about keeping it has already answered.

### Principle of Examined assumptions

Examined assumptions asks that a judgment take the current state as given only where there is a reason to do so. What a judgment takes as given is its assumption. Such judgments include those about placement, classification, naming, scope, and whether something should exist. The current state includes who uses a thing, what the existing text says, and how the existing structure divides its subject. The current state is what has accumulated so far, and much of it is neither guaranteed nor decided.

The test is to name what the judgment takes as given, and then to state why it can be taken as given. That it is the current state is not a reason. Examples of a reason are a contract, an invariant, and readers who rely on a shared form.

Where structure and content conflict, the same test decides which of the two is adjusted. Where the structure can be taken as given, the unit conforms, a case that the Sibling consistency principle governs. Where it cannot, the structure is revised, a case that the Structural fix principle governs.

A judgment that fails the test shows in its result. For example, a judgment about placement results in the "wrong home" antipattern. A judgment about a name or a description results in the "caller-bound framing" antipattern, and a judgment about the scope of an edit results in the "under-scoped change" antipattern.

The **"snapshot reasoning"** antipattern is a judgment that takes the current state as fixed, although that state can change. Examples of such a state are who calls a thing, what references it, which inputs occur, and how a document under review is currently divided. The work itself can change the state, and so can a later change that the result has to survive. In the clearest case, work on a thing takes the current form of that same thing as its ground.

### Principle of Applicability

Applicability asks that what is already established be applied to a case according to the reason for which it was established. Examples of what is established are a rule, a decision, a criterion, and a named concept. Neither the wording of what is established nor the appearance of the case decides which cases it covers.

The test is to state the reason, and then to show that the reason holds in the case. Where the reason cannot be stated, what is established is not yet understood well enough to be applied.

A case can resemble the established wording, or resemble a case that is already covered. Either resemblance is weak evidence that the reason holds. The writer describes the case while reading the established wording, so the description adopts that wording. The case is therefore first described without the established wording.

The **"false analogy"** antipattern applies what is established to a case on the ground of resemblance, without checking that the reason holds there. What is established can be correct and exactly quoted, so nothing on the page marks the error.

## What is already there

### Principle of Sibling consistency

Sibling consistency asks that a unit match the form that its siblings already share: density, tone, vocabulary, language, structure, naming convention, argument order, and a module's layout. That shared form is information that readers carry from one sibling to the next. A locally better choice in one of them costs the whole set.

The test is to state the form that the siblings share before writing the unit. A unit written without that statement conforms only by accident.

Where the shared structure allows variation, such as optional sections or permitted merging, using that flexibility is conforming. Where the material does not fit even then, the mismatch is reported rather than resolved silently in one place, a case that the Visible conflict principle governs.

The **"sibling mismatch"** antipattern is a unit whose form departs from its siblings. The usual forms are a list item several times the length of those around it, a passage written in a different language from the document, and a module laid out unlike its neighbors. The nearest sibling set is the document itself, and the terms that it has already used are part of the form that it shares. A second term for a concept that the document has already named reads as a second concept.

The principle can be overapplied by condensing text to match sparser siblings instead of removing what does not earn its place under the Necessity principle.

### Principle of Discovery

Discovery asks that what already exists be found first, and that the addition be related to it. The search extends beyond the unit under edit. This is because an existing type for the same concept usually lives in another module. An existing statement of the same fact usually lives in another document.

The test is to name what the addition relates to: the element that it extends, specializes, replaces, or deliberately sits beside. When the addition can name none of these, the writer has usually not searched.

Without a relation between the existing element and the addition, improvements never propagate from one to the other. Readers are left to determine for themselves how the two relate.

The **"unintegrated addition"** antipattern is a new element placed without being related to the existing model. It takes several forms: something that overlaps what is already there, a specialization of an existing general type defined as though unrelated, and a section written in its own vocabulary and classification rather than the one already in use. The degree of overlap is not the point, and an exact duplicate is only the most extreme case.

### Principle of Visible conflict

Visible conflict asks that a mismatch between two things that must correspond be reported rather than hidden. In code, an invariant that the type system cannot enforce is asserted rather than hidden. That is the established fail-fast principle. In documentation, when a description and the thing that it describes disagree, the conflict is reported rather than resolved by quietly moving one side to match the other.

The test is whether a violation would reach someone who can act on it.

The **"silent fallback"** antipattern hides a violation of an invariant that should have held, such as returning a default, suppressing an error, or substituting null. It then proceeds as though nothing happened.

The **"silent contradiction"** antipattern is an unreported disagreement between a description and the thing that it describes.

## What lies outside the boundary

### Principle of Reader's position

Reader's position asks that output start from what the reader can see rather than from what the writer knows. The first judgment is who the reader is and where they stand, because everything that follows is weighed from that position. The writer holds three things that the reader does not, and each affects the output in its own way:

- **Boundary.** The writer sees internals and current callers. The reader has only what the boundary exposes. The Contract principle governs this concern.
- **Session.** The writer holds instructions, discussion, and the previous version. The reader has none of it. The Session-blind principle governs this concern.
- **Namespace.** The writer knows the referent and can read a loose term back. The reader can follow only terms that resolve in a public namespace. The Words that resolve principle governs this concern.

A symbol name and a doc comment are addressed to the same reader, and the same three concerns govern both.

### Principle of Contract

Contract asks that a name or a description say what its boundary promises to the outside, and nothing besides. The placement of the name or the description decides which boundary applies. A public function's name and doc comment stand at the public boundary, a comment inside the body stands within it, and a design note inside a module stands at the module's edge. The Contract and Placement principles converge here: the subject fixes the home, and the home fixes the boundary.

The test is whether the name or the description can be restated as a promise with the thing itself as its subject. A mechanism enters the promise only by being fixed at the boundary, where it stops being free to change and so stops being internal. Facts about current callers are observable from outside but are not what the thing promises.

Names and descriptions addressed to internal readers are outside this principle. Where an implementation note belongs is a question for the Placement principle. Crossing the boundary outward has one use: explaining behavior that is observable from outside and otherwise surprises.

Two antipatterns cross this boundary in opposite directions.

The **"exposed internals"** antipattern carries implementation concepts across the boundary to a reader who cannot see them and never asks about them.

The **"caller-bound framing"** antipattern names or describes a thing from its current caller's viewpoint rather than by what the thing promises.

### Principle of Session-blind

Session-blind asks that the work be composed for a reader who never saw this session. Its material is what the session led the writer to understand, not the session itself. A work composed from the session itself keeps what the session made prominent and relies on what the session made obvious.

The test is whether a writer who reached the same understanding without this session would state this fact, use this framing, give this point this weight, reach for this example, and choose these words.

The session is not only what is visible in the conversation. A line of reasoning that occurred only in the writer's own thinking counts too.

Mentioning a rejected alternative always looks justifiable from inside the session. It earns a place only when a fresh reader would expect that alternative and the choice needs explaining. The test is whether a reader who has never heard of the alternative benefits from being told about it.

The **"session leak"** antipattern is a trace of the session in the output: an instruction mixed in, a rejected alternative mentioned, and a pointer to something that exists only for this session.

The **"salience leak"** antipattern is the session increasing the emphasis that a point receives or narrowing the example chosen for it. The same pressure also lowers the weight of what the session did not raise. A point can lose its place entirely, not just its emphasis.

The **"context-bound statement"** antipattern is a statement that takes its meaning from the context in which it was made. It is written for a reader who does not have that context. Examples of such a context are the question that a remark answered, the case that a decision settled, and the conditions under which a result was obtained. The reader takes the statement to mean something else, usually something more general or more certain, and nothing on the page shows the difference. What repairs the statement is wording that gives a reader outside the context the meaning that the statement had inside it. That wording can be a narrower claim or a different claim, and sometimes the repair is to drop the statement.

### Principle of Version-blind

Version-blind asks that the current state be described by a writer who never saw the previous one. It specializes the Session-blind principle to the part of the session that is the old version. The artifact's purpose decides whether the principle applies. A document that exists to record a decision or an event carries the history that it exists to record.

Two failures take opposite forms, and writing as though the old version had never existed prevents both.

The **"history leak"** antipattern writes as though the reader shares the old version. A qualifier that describes the current state relative to a previous one is not empty, which is what lets it survive: the change is still vivid to the writer, and even a fresh audit can rate the comparison informative. The failure is on the reader's side, because a reader who is not reading for history wants the current state alone.

The **"unsolicited history"** antipattern does the opposite. It narrates the change for a reader assumed not to know it: what replaced what, what a thing used to be called. It is typically well-intentioned. It also fails the Necessity principle, because the added history does not earn its place.

### Principle of Words that resolve

Words that resolve asks that every term and phrase reach a meaning that the reader already has access to. A meaning is accessible when the reader can reach it through a namespace that the reader already holds, without asking the writer. For the reader of a project's artifacts, such namespaces include the code symbols of the repository, the established terms of the domain, and the terms explicitly defined in the project's tracked documents.

The requirement extends beyond single terms to phrases, where the reader must be able to recover the relationship between the words. It also extends past the phrase, to the relation between adjacent elements and to the status that a statement or an enumeration holds, such as rule or example.

The test is whether the reader can recover every meaning, relation, and status without asking the writer.

Two practices keep terms resolvable. A concept that recurs often enough within one document to want a name resolves after it is defined at first use. However, a name intended to persist beyond the document is a design decision that belongs to the user.

Conversely, an unfamiliar term already in the repository is worth checking for a definition before it is adopted. In a document, a term is defined where a passage states what it means. Any other appearance is a use, and a prominent use, such as a heading or a section topic, is still a use. That existing text uses a term is a weak reason to adopt the term. This is because the term is often an earlier session's coinage that further use establishes as vocabulary.

Relations and statuses resolve by being stated. An enumeration needs the text around it to show what it enumerates, usually in the sentence that introduces it or contains it. An enumeration also needs its status shown. A closed set is enumerated in full. For an open set, the abstract statement comes first, and the enumeration that follows is explicitly marked as examples. Precision comes from explicit markers, such as which quantifier applies, how far the scope reaches, whether the set is open or closed, and whether a passage defines or illustrates. It does not come from adding items to an enumeration.

In code, a concept that only a general-purpose word names is usually one whose contract or abstraction level is not yet defined. The Structural naming principle governs this case.

The **"unmarked coinage"** antipattern is a term that the reader cannot follow to any referent. It usually arrives in one of two ways: a private label from the session enters the output, or condensing an expression produces a compound that the writer can understand only because they already know the meaning. Typography can strengthen the impression that the term has a referent, because backticks suggest a code symbol and capitalization suggests a defined term. However, the failure lies in the term rather than the markup.

The **"catch-all name"** antipattern makes a general-purpose word the fixed name of one specific concept. The name then resolves only to the word's broad everyday sense, and the reader cannot distinguish it from its neighbors. Using such a word in its plain generic sense, where context fixes the referent, is not this failure.

The **"stripped term"** antipattern drops the qualifier from a defined term at a later mention, leaving a bare word that resolves only to its everyday sense. The full term belongs wherever it names its concept, and shortening is safe only where context has already fixed the referent.

The **"packed phrase"** antipattern condenses an expression until the reader can no longer recover how its words relate. Each word can resolve on its own, and what the condensation drops is the relation between them.

The **"unstated relation"** antipattern presents a relation or a status through form alone, such as juxtaposition, a parenthesis, or position in the prose, instead of stating it in words. The relation is known only to the writer, the reader supplies one by inference, and a wrong inference shows nothing on the page. An unlabeled example or an unmarked enumeration is read in the strongest way, as a rule or a complete classification. Forms include parentheses with unstated relations, defined names without the kind of thing they name, unlabeled example sentences, and enumerations that do not show whether they are complete.

The **"assumed connection"** antipattern refers to something elsewhere in the text as though the reader already had it in view. A point restated in different wording is such a reference when the reader cannot recognize it as the same point. The writer holds the whole text at once, while the reader has only what the words identify. The target is reachable only where the wording, the distance, and the number of candidates leave one reading. A short reference is not itself a failure when the reader can determine what it means at that point.

The **"culture-bound phrase"** antipattern is wording that resolves only through its language's figurative and idiomatic conventions. Those conventions are not a namespace that every reader holds. The meaning does not survive translation or reach readers whose native language differs. A reader of the original language recovers the meaning, and the wording therefore passes a check that asks only for a referent. Personification is one form. Another is a figurative chain, where successive sentences use words from one figurative system. A figurative term established in the domain resolves through that domain's vocabulary and is not this failure.

### Principle of Structural naming

Structural naming asks that a name be treated as a claim about the structure rather than as a label to swap. A name declares what a concept presents to its scope, and it also has to distinguish that concept from the ones beside it. A bad name is a symptom of one of three causes, worth suspecting in this order:

1. The concept is not sufficiently defined, a case that the Structural fix principle governs. The name is being asked to carry a distinction that the design does not express. Restructuring the region sometimes removes the thing that needed naming.
2. The name does not state what the thing promises, a case that the Contract principle governs. In its place stands either the caller's viewpoint, named the "caller-bound framing" antipattern, or an internal fact, named the "exposed internals" antipattern. A third possibility is a name that states only what the thing is in itself. Such a name is true, and it still does not fix the role that the thing plays for its scope.
3. The name does not identify its referent, a case that the Words that resolve principle governs. It is either too broad, named the "catch-all name" antipattern, or opaque, named the "unmarked coinage" antipattern.

In code, causes 1 and 2 come first, and a simple rename rarely suffices. In documentation, the surface fix at cause 3 is often enough.

## Companion skills

Japanese has failure modes beyond what the preceding sections cover. The `fal-write-ja` skill owns these failure modes. Whenever the output is Japanese, that skill applies together with this one.

Having applied these concepts while writing does not make checking the finished work redundant. Believing otherwise is itself a self-evaluation from the writer's position, which the Reader's position principle warns about. The `fal-agentic-audit` skill supplies that separate process.
