---
name: fal-agentic-vocabulary
description: A shared vocabulary between the user and the agent for work performed by LLM agents. The vocabulary is made of named principles and antipatterns that cover the artifacts of the work, such as documentation, comments, software design decisions, conceptual models, and plans. Useful for learning what the user cares about before substantial writing or design work. Useful for understanding the intent of feedback that names or implies one of these concerns. Useful in changes that look simple but affect structure, and where style or quality is not the stated concern. Counters characteristic failures of LLM-generated output.
---

# Agentic vocabulary

This skill names the concerns that recur in the artifacts that LLM agents produce. Documentation, code comments, and software design decisions are such artifacts. So are the more abstract structures that the work is based on, such as a conceptual model, a classification, or a plan. The vocabulary supplies terms for judging whether a principle applies to a particular case, rather than rules that determine the outcome.

Six biases of LLM-generated output cause the failures named here. Each bias is a default that the vocabulary counteracts:

- **Anchoring bias.** The existing visible structure and state over-determine the output. They are taken as given, and the output is made to conform to them.
- **Local bias.** The writer examines only the unit under edit. The writer does not consult anything outside that unit, such as its siblings, the structure above it, and the thing that it must correspond to.
- **Additive bias.** Generating costs the writer almost nothing. Reading and maintaining are costly. The writer adds material without the selection, reuse, and reduction that should precede an addition.
- **Contextual bias.** The writer writes from the working context without rebuilding the content for a reader who does not share that context. What the context made prominent is written even where the reader does not need it. What the context made obvious is treated as known to the reader.
- **Compression bias.** The writer pursues brevity by condensing the expression rather than by selecting what to keep.
- **Defensive bias.** The writer avoids breaking, removing, omitting, and admitting error in favor of adding, keeping, accepting without reporting, and defending in advance.

Each principle has a section of its own. The section states the principle and what it requires of the writer, together with a test where the principle has one. Each antipattern is defined in the section of the principle that primarily prevents it, and other sections refer to it by name.

The vocabulary applies throughout the work, whether or not anything invokes it.

A difficulty in the work is one occasion for consulting the vocabulary. A problem that local adjustment does not solve is usually a sign of one of the failures named in the sections that follow. Examples include a name that remains unsatisfactory, content that does not fit the structure, and an urge to qualify a statement or to keep material. The response is to consult the definition rather than to continue the local adjustment. A critique that questions one of the work's premises counts as the same kind of signal.

Feedback from the user is another occasion for consulting the vocabulary. A remark can name a concern directly, or describe only its symptom. For example, a complaint about a name is usually about the structure that the name is meant to express, a case to which the Structural naming principle applies. Any concern in the vocabulary can be raised this way.

Under the Applicability principle, an antipattern name applies when the defined failure occurs, not merely when the case resembles a form mentioned in the definition. Each name identifies one kind of problem, so several names can apply to one case.

## What the unit contains

### Principle of Necessity

Necessity requires that anything added be worth its cost. The cost is the reader's attention now and the project's maintenance later. The default is to leave it out. Content is worth its cost when the reader needs it and cannot get it from what is already there. That content is true, relevant, or not yet said does not make it worth its cost. In documentation, such content is most often a contract or an explanation of a reason that is not obvious.

The test is removal: take the content out, and name what the reader then loses.

Brevity comes from removing what is not worth its cost, not condensing it.

Being asked to make something clearer does not justify an addition. When the text already states the point, an addition more often obscures the point than clarifies it, and the first remedy is rewriting what is there.

The **"over-documentation"** antipattern is content, in a document or a comment, that is not worth its cost. Two of its forms have names of their own: the "redundant statement" antipattern and the "unsolicited clarification" antipattern.

The **"redundant statement"** antipattern restates what the artifact already says, and the copy becomes inaccurate as the original changes. Two passages are copies when either one would answer the same question for the same reader. Sharing a subject does not make two passages copies.

The **"unsolicited clarification"** antipattern is a passage written to prevent a misreading, an objection, or a doubt, none of which the reader would otherwise have. Its forms include a justification, a negation of an alternative, a contrast with a neighboring concept, a statement of what is out of scope, a caveat, and a concession. The clarification introduces what it is meant to prevent.

The **"staleness surface"** antipattern is content that a routine change can make false without anyone noticing. Where the same point can be made one abstraction level up, that form remains accurate after the change.

The **"overlong block"** antipattern is a paragraph, list item, function, or code block that is longer than its role requires. Length is what is visible, and the cause is elsewhere: content that is not worth its cost, a unit that has more than one role, or detail that belongs in a smaller scope. The content question is decided first, because splitting content that should have been removed is wasted work.

Applying the principle too little is the more frequent failure. The principle can also be overapplied. Applying it means judging how much to keep, not keeping as little as possible. Overapplication removes content that is worth its cost, such as the following:

- A statement that gives what the reader genuinely lacks, however short the text would be without it
- A qualifier that would make the statement false if removed
- A sentence that adds no new fact but prevents the "unstated relation" antipattern by stating a relation or a status where the reader needs it
- A repeated noun or a helper word, such as "that" or "then," that determines how a phrase is read

### Principle of Unit focus

Unit focus requires that a unit have one role, at whatever level the unit is. In documentation, examples of units are a sentence, a paragraph, a list, a section, and a document. In code, examples are a function, a type, and a module. Units come to have more than one role in two ways. In one, the structure chosen while the unit was small remains unchanged as content accumulates inside it. In the other, content that would fill two units is condensed into one for brevity.

The test is naming: can the unit's role be stated in one phrase, without an "and" joining two roles?

Notation follows from the role. A sentence that states two thoughts becomes two sentences. A few short items stay inline in a sentence, and list notation begins where the inline form stops being readable. A list whose items are not genuinely parallel is rebuilt from the question of what is being enumerated, or converted back to prose. In code, orchestration and low-level manipulation sharing one body obscure each other.

A split need not follow the unit's current boundaries. Where connected roles cannot be separated within the current boundaries, the surrounding region is recomposed from its roles, and the unit's boundaries change accordingly.

The **"split focus"** antipattern is a unit having more than one role or concern. It is independent of length. A unit with a single role can still be too long, a failure named the "overlong block" antipattern.

The principle can be overapplied by splitting what belongs together. A connective that states how its clauses relate is part of the content, not a point at which to split. Where such a sentence is split for length, the split is complete only when the resulting sentences still state the relation. Connected reasoning rewritten as bullet points loses the relations in the same way. A tiny helper extracted from a function requires readers to consult it to understand the parent's flow.

## What determines the unit

### Principle of Placement

Placement requires that a unit be in the place that covers its subject, at the abstraction level of that place. A unit is about something, and a place covers something. For example, a comment covers the declaration that it is attached to, a section covers its topic, and a module covers its responsibility. A unit outside the place that covers its subject fails in two ways. The change that falsifies the unit happens elsewhere, so nothing prompts its update. The reader looks for the unit elsewhere, or takes it as a property of the place where it is.

The test is to name what the unit is about, and then to name what the place covers. The unit belongs where the two match.

The place that covers a unit's subject also determines an abstraction level. A module header states its responsibilities, a function doc states its contract, and a type doc states its role. Detail about a smaller thing belongs in the smaller scope, next to that thing. Orientation material belongs where a newcomer looks first.

In documentation, where a document set is split into overview and detail, such as a skill file and its references or a README and `docs/`, each kind of material already has an assigned place. Worked examples and per-item explanations belong to the detail layer.

The same matching applies in code. Logic belongs where its subject is implemented, and a caller that has to access another module's internals indicates a boundary defined in the wrong place. When responsibilities share a location, the question is whether they share a subject or only a place: responsibilities placed together accidentally appear to be placed together deliberately.

The subject of a constraint is whoever guarantees it. A rule that the module itself guarantees belongs in its own types. By contrast, a rule that only one use site imposes belongs in that use site's layer, stated with that use site as its subject.

The **"misplacement"** antipattern is a unit that is not in the place that covers its subject, at the abstraction level of that place. The urge to repeat a caveat "to be safe" is usually this antipattern: the caveat is outside the place that covers its subject, or two scopes overlap.

The **"unabstracted detail"** antipattern is a misplacement by abstraction level. The unit's subject is part of what the place covers, but the unit states it in finer detail than the level of that place.

### Principle of Structural fix

Structural fix requires that a difficulty be traced to its cause before it is addressed. Difficulty in writing, naming, or fitting content into a structure is a symptom, and the cause is usually one level of structure above. An awkward API indicates a problem in the underlying model rather than in the API itself.

The test is to ask what would have to be different one level up for the difficulty to disappear. A concrete answer is a hypothesis to check rather than a conclusion.

The **"wrong-layer patch"** antipattern works at the symptom layer and leaves the structural cause unaddressed. Each of the following substitutes for the structural fix: filling an empty section with text so that every heading has a paragraph, adding a run-time check for a state that the types could have excluded, and writing prose to compensate for a structural awkwardness.

### Principle of Redesign

Redesign requires that a structure be rebuilt to be coherent under changed assumptions rather than patched under the old ones. It specializes the Structural fix principle. The difference is the trigger: the Structural fix principle starts from a visible symptom, and the Redesign principle starts from a premise that has changed. The result reads as if it had always been this way.

The test is whether a writer who never saw the previous version would define these same boundaries, sections, and types.

A redesign can name what it makes unnecessary: the functions that it replaces, the helpers that it generalizes, and the special cases that it handles as part of the general case. Every existing element needs that decision, and having read the existing code is not the same as having decided what it should become.

During the work, the signal is the urge to make the content fit the structure that is already there. The urge means that the assumptions have already changed, and the change requires rederiving the affected scope from its responsibilities.

The **"under-scoped change"** antipattern is a change whose extent is determined by the old structure rather than by the changed assumptions. It specializes the "wrong-layer patch" antipattern: here the symptom layer is the part already under edit. Its most frequent form is an insertion into the unchanged structure: adding a flag, wrapping with a condition, or inserting a special case.

The **"vestige"** antipattern is a remnant, such as dead code, superseded structure, or an assumption that no longer applies, that the current design has made unnecessary. It is an under-scoped change seen from its result: some edit omitted the deletions that the design required, and that edit need not be the current change. "Vestigial" means superseded, not merely unexercised: code that nothing reaches today still belongs when the design has a reason for it.

When the change required replacement, the **"needless backward compatibility"** antipattern keeps the old interface alongside the new one. It is an under-scoped change made deliberately: where a vestige remains because it was overlooked, here the old interface is kept deliberately. The trigger is the writer's own thought that the callers must not break, substituting for a decision that was not made. The user decides whether the old interface is kept. Where the user requests the change and does not ask to keep the old interface, the old interface is removed.

### Principle of Examined assumptions

Examined assumptions requires that a judgment take something as given only where there is a reason to do so. What a judgment takes as given is its assumption. Such judgments include those about placement, classification, naming, scope, and whether something should exist. Examples of an assumption are the current state and a consideration's role as a condition for the conclusion.

The current state includes who uses a thing, what the existing text says, and how the existing structure divides its subject. The current state is what has accumulated so far, and much of it is neither guaranteed nor decided.

A judgment that treats a consideration as a necessary or sufficient condition for its conclusion assumes that the consideration can have that role. A consideration can be true and relevant without being such a condition. A consideration has that role only where the purpose or the constraints of the work justify its use in that role, and those constraints include the criteria that the work has established.

The test is to name what the judgment takes as given, and then to state why it can be taken as given. That it is the current state is not a reason. Neither is that a critique raised the consideration or that the consideration is easy to check. Examples of a reason are a contract, an invariant, and readers who rely on a shared form.

Where structure and content conflict, the same test determines which of the two is adjusted. Where the structure can be taken as given, the unit conforms, a case to which the Sibling consistency principle applies. Where it cannot, the structure is revised, a case to which the Structural fix principle applies.

A judgment that fails the test is visible in its result. For example, a judgment about placement results in the "misplacement" antipattern. A judgment about a name or a description results in the "caller-bound framing" antipattern, and a judgment about the scope of an edit results in the "under-scoped change" antipattern.

The **"snapshot reasoning"** antipattern is a judgment that takes the current state as fixed, although that state can change. Examples of such a state are who calls a thing, what references it, which inputs occur, and how a document under review is currently divided. The work itself can change the state, and so can a later change after which the result has to remain correct. In the clearest case, work on a thing takes the current form of that same thing as its basis.

The **"unwarranted condition"** antipattern treats a consideration as a necessary or sufficient condition for the conclusion of a judgment. In this failure, neither the purpose nor the constraints of the work justify using the consideration in that role. One cause is a visible fact about the existing structure. Examples are where a piece of information is written and whether another item already covers a case. The existing structure then over-determines the judgment, and the fact is used as a condition that nothing warrants. Where the "snapshot reasoning" antipattern treats a fact as fixed, this antipattern treats a fact as a condition, however stable the fact is.

### Principle of Applicability

Applicability requires that what is already established be applied to a case according to the reason for which it was established. Examples of what is established are a rule, a decision, a criterion, and a named concept. Neither the wording of what is established nor the appearance of the case determines which cases it covers.

The test is to state the reason, and then to show that the reason applies to the case. Where the reason cannot be stated, what is established is not yet understood well enough to be applied.

A case can resemble the established wording, or resemble a case that is already covered. Either resemblance is weak evidence that the reason applies. The writer describes the case while reading the established wording, so the description adopts that wording. The case is therefore first described without the established wording.

The **"false analogy"** antipattern applies what is established to a case because the case resembles the established wording or a case already covered, without checking that the reason applies there. What is established can be correct and exactly quoted, so nothing in the text shows the error.

## What is already there

### Principle of Sibling consistency

Sibling consistency requires that a unit match the form that its siblings already share: density, tone, vocabulary, language, structure, naming convention, argument order, and a module's layout. That shared form is information that readers learn from one sibling and apply to the next. A choice that is better for one sibling alone makes that information unreliable for the whole set.

The test is to state the form that the siblings share before writing the unit. A unit written without that statement conforms only accidentally.

Where the shared structure allows variation, such as optional sections or permitted merging, using that flexibility is conforming. Where the material does not fit even then, the mismatch is reported rather than resolved silently in one place, a case to which the Visible conflict principle applies.

The **"sibling mismatch"** antipattern is a unit whose form departs from its siblings. The usual forms are a list item several times the length of those around it, a passage written in a different language from the document, and a module laid out unlike its neighbors. The nearest sibling set is the document itself, and the terms that it has already used are part of the form that it shares. A second term for a concept that the document has already named reads as a second concept.

The principle can be overapplied by condensing text to match sparser siblings instead of removing what is not worth its cost under the Necessity principle.

### Principle of Discovery

Discovery requires that what already exists be found first, and that the addition be related to it. The search extends beyond the unit under edit. This is because an existing type for the same concept is usually defined in another module. An existing statement of the same fact is usually written in another document.

The test is to name what the addition relates to: the element that it extends, specializes, replaces, is merged into, or is deliberately placed beside. When none of these can be named, the writer has usually not searched.

During the work, the signal is a plan or a proposal stated only as adding something. Such a statement does not show whether the relation that the test requires has been decided.

Without a relation between the existing element and the addition, improvements never propagate from one to the other. Readers are left to determine for themselves how the two relate.

The **"unintegrated addition"** antipattern is a new element placed without being related to the existing model. It takes several forms: something that overlaps what is already there, a specialization of an existing general type defined as though unrelated, and a section written in its own vocabulary and classification rather than the one already in use. The degree of overlap does not define the failure. An exact duplicate is only the most extreme case, and elements that model different concepts can look alike and still diverge once their relation is decided.

### Principle of Visible conflict

Visible conflict requires that a mismatch between two things that must correspond be reported rather than hidden. In code, an invariant that the type system cannot enforce is asserted rather than hidden. That is the established fail-fast principle. In documentation, when a description and the thing that it describes disagree, the conflict is reported rather than resolved by changing one side to match the other without reporting the change.

The test is whether a violation would be reported to someone who can act on it.

The **"silent fallback"** antipattern hides a violation of an invariant that should have held, such as returning a default, suppressing an error, or substituting null. It then proceeds as though nothing happened.

The **"silent contradiction"** antipattern is an unreported disagreement between a description and the thing that it describes.

## What is outside the boundary

### Principle of Reader's position

Reader's position requires that output start from what the reader can see rather than from what the writer knows. The first judgment is who the reader is and what they already have, because everything that follows is judged from that position. The writer has three things that the reader does not, and each affects the output in its own way:

- **Boundary.** The writer sees internals and current callers. The reader has only what the boundary exposes. The Contract principle addresses this concern.
- **Session.** The writer has instructions, discussion, and the previous version. The reader has none of it. The Session-blind principle addresses this concern.
- **Namespace.** The writer knows the referent and can interpret an imprecise term correctly. The reader can understand only terms that resolve in a public namespace. The Words that resolve principle addresses this concern.

A symbol name and a doc comment are addressed to the same reader, and the same three concerns apply to both.

### Principle of Contract

Contract requires that a name or a description say what its boundary promises to the outside, and nothing besides. The placement of the name or the description determines which boundary applies. A public function's name and doc comment are at the public boundary, a comment inside the body is within it, and a design note inside a module is at the module's boundary. The Contract and Placement principles connect here: the subject determines the place, and the place determines the boundary.

The test is whether the name or the description can be restated as a promise with the thing itself as its subject. A mechanism becomes part of the promise only when the boundary guarantees it. It then can no longer change freely, so it is no longer internal. Facts about current callers are observable from outside but are not what the thing promises.

Names and descriptions addressed to internal readers are outside this principle. Where an implementation note belongs is a question for the Placement principle. Stating an internal fact at the boundary has one use: explaining behavior that is observable from outside and otherwise surprises.

Two antipatterns violate this boundary in opposite directions.

The **"exposed internals"** antipattern states implementation concepts to a reader outside the boundary, who cannot see them and never asks about them.

The **"caller-bound framing"** antipattern names or describes a thing from its current caller's viewpoint rather than by what the thing promises.

### Principle of Session-blind

Session-blind requires that the work be composed for a reader who never saw this session. Its material is the understanding that the writer gained in the session, not the session itself. A work composed from the session itself keeps what the session made prominent and relies on what the session made obvious.

The test is whether a writer who gained the same understanding without this session would state this fact, use this framing, give this point this emphasis, select this example, and choose these words.

The session is not only what is visible in the conversation. A line of reasoning that occurred only in the writer's own thinking counts too.

Mentioning a rejected alternative always looks justifiable from inside the session. It is worth its cost only when a fresh reader would expect that alternative and the choice needs explaining. The test is whether a reader who has never heard of the alternative benefits from being told about it.

The **"session leak"** antipattern is a trace of the session in the output: an instruction restated, a rejected alternative mentioned, and a pointer to something that exists only for this session.

The **"salience leak"** antipattern is the session increasing the emphasis that a point receives or narrowing the example chosen for it. The same effect also reduces the emphasis on what the session did not raise. A point can be omitted entirely, not only de-emphasized.

The **"context-bound statement"** antipattern is a statement whose meaning depends on the context in which it was made. It is written for a reader who does not have that context. Examples of such a context are the question that a remark answered, the case that a decision settled, and the conditions under which a result was obtained. The reader takes the statement to mean something else, usually something more general or more certain, and nothing in the text shows the difference. The fix for the statement is wording that gives a reader outside the context the meaning that the statement had inside it. That wording can be a narrower claim or a different claim, and sometimes the fix is to remove the statement.

### Principle of Version-blind

Version-blind requires that the current state be described by a writer who never saw the previous one. It specializes the Session-blind principle to the part of the session that is the old version. The artifact's purpose determines whether the principle applies. A document that exists to record a decision or an event contains the history that it exists to record.

Two failures take opposite forms, and writing as though the old version had never existed prevents both.

The **"version-bound description"** antipattern describes the current state relative to a previous one, as though the reader shares that previous version. Such a description is not empty, which is why it tends to remain after review: the change is still vivid to the writer, and even a fresh audit can rate the comparison informative. The failure affects the reader, because a reader who is not reading for history wants the current state alone.

The **"unsolicited history"** antipattern does the opposite. It narrates the change for a reader assumed not to know it: what replaced what, what a thing used to be called. It is typically well-intentioned. It also fails the Necessity principle, because the added history is not worth its cost.

### Principle of Words that resolve

Words that resolve requires that every term and phrase resolve to a meaning that the reader already has access to. A meaning is accessible when the reader can find it through a namespace that the reader already has, without asking the writer. For the reader of a project's artifacts, such namespaces include the code symbols of the repository, the established terms of the domain, and the terms explicitly defined in the project's tracked documents.

The requirement extends beyond single terms to phrases, where the reader must be able to understand the relationship between the words. It also extends past the phrase, to the relation between adjacent elements and to the status that a statement or an enumeration has. For example, the reader can tell whether a statement is a rule or an example, and whether it is a checked fact or an inference.

The test is whether the reader can understand every meaning, relation, and status without asking the writer.

A definition alone does not show that a term resolves, because the reader must still be able to understand the definition. Once the term resolves, the Defined terms principle is used to judge whether the definition is needed and whether the name and the definition are well made.

An unfamiliar term already in the repository is worth checking for a definition before it is adopted. In a document, a term is defined where a passage states what it means. Any other appearance is a use, and a prominent use, such as a heading or a section topic, is still a use. That existing text uses a term is a weak reason to adopt the term. This is because the term is often an earlier session's coinage that further use establishes as vocabulary.

For relations and statuses, the default is to use short connective phrases or labels. Examples follow the general statement that they illustrate and are marked as examples. The sentence that introduces or contains an enumeration states what is being listed. It also shows whether the writer intends to list all members or give examples.

In code, a concept that only a general-purpose word names is usually one whose contract or abstraction level is not yet defined. The Structural naming principle applies to this case.

The **"unmarked coinage"** antipattern is a term for which the reader cannot find any referent. It usually appears in one of two ways: a private label from the session is written into the output, or condensing an expression produces a compound that the writer can understand only because they already know the meaning. Typography can strengthen the impression that the term has a referent, because backticks suggest a code symbol and capitalization suggests a defined term. However, the failure is in the term rather than the markup.

The **"catch-all name"** antipattern makes a general-purpose word the fixed name of one specific concept, where the name causes either of two problems. In one, the name conveys only the word's broad everyday sense, and the reader cannot determine the needed distinction from the name and the context where it is used. In the other, the document also needs the word's everyday sense within the scope of the name, and a use in that sense conflicts with the name. A definition lets the reader look up the referent, but it solves neither problem. A general-purpose word used only in its everyday sense, where context determines the referent, is not this failure.

The **"stripped term"** antipattern removes the qualifier from a defined term at a later mention, leaving an unqualified word that resolves only to its everyday sense. The full term belongs wherever it names its concept, and shortening is safe only where context has already determined the referent.

The **"packed phrase"** antipattern condenses an expression until the reader can no longer determine how its words relate. Each word can resolve individually, and what the condensation loses is the relation between them.

The **"unstated relation"** antipattern relies on form to convey a relation or status that the intended reader's context and conventions do not establish. Such forms include juxtaposition, parentheses, and position in prose. Form that presents as supplementary the information that a claim depends on is also this failure.

The **"unclear reference"** antipattern refers to something elsewhere in the text as though the reader already knew what it is. A point restated in different wording is such a reference when the reader cannot recognize it as the same point. The writer knows the whole text, while the reader has only what the words identify. The reader can identify the target only where the wording, the distance, and the number of candidates allow one reading. A short reference is not itself a failure when the reader can determine what it means at that point.

The **"culture-bound phrase"** antipattern is wording that resolves only through its language's figurative and idiomatic conventions. Those conventions are not a namespace that every reader has. Translation does not preserve the meaning, and readers whose native language differs do not understand it. A reader of the original language understands the meaning, and the wording therefore passes a check that requires only a referent. Even for that reader, the wording can leave unclear which operation or fact it means. Such wording typically uses a word outside its primary sense. Personification is one form. Another is a figurative chain, where successive sentences use words from one figurative system. A figurative term established in the domain resolves through that domain's vocabulary and is not this failure.

### Principle of Structural naming

Structural naming requires that a name be treated as a claim about the structure rather than as a label to swap. A name states what a concept provides to its scope, and it also has to distinguish that concept from the ones beside it. A bad name is a symptom of one of three causes, worth suspecting in this order:

1. The concept is not sufficiently defined, a case to which the Structural fix principle applies. The name is expected to express a distinction that the design does not express. Restructuring the region sometimes removes the thing that needed naming.
2. The name does not state what the thing promises, a case to which the Contract principle applies. Instead, the name states either the caller's viewpoint, named the "caller-bound framing" antipattern, or an internal fact, named the "exposed internals" antipattern. A third possibility is a name that states only what the thing is in itself. Such a name is true, and it still does not determine the role that the thing has in its scope.
3. The name does not identify its referent, a case to which the Words that resolve principle applies. It is either too broad, named the "catch-all name" antipattern, or opaque, named the "unmarked coinage" antipattern.

In code, causes 1 and 2 are examined first, and a simple rename rarely suffices. In documentation, the fix at cause 3 alone is often enough.

### Principle of Defined terms

Defined terms requires that a term be defined when the reader needs a distinction that its wording and context do not establish. The definition states that distinction accurately within a scope that the reader can recognize. Its value is compared with the effort of learning and consulting it, and with any restriction that it places on using the term in its everyday sense. Recurrence alone does not establish the need for a definition.

A definition is judged by four questions. A later answer can change an earlier one. For example, a narrower scope under question 4 can make a definition worth its cost under question 1.

1. **Is a definition needed?** A definition that explains an established term is worth its cost where the reader lacks the term and needs it. Before the document stipulates a meaning of its own, ordinary wording is tried. Examples are a qualifier that states what the thing belongs to, such as "detection candidate" for "candidate," and a relation written out where the concept is used. A term of several words needs a definition only for the part of its meaning that its words and their relation do not already state. The test is removal: take the definition out, read each use, and name what the reader then loses.
2. **Does the name fit?** A term that the domain establishes or the project defines for the same concept is preferred. A new name fits where it lets the reader infer roughly what the concept is about. A compound is readable only where its head already resolves at the point of use. A general-purpose word as the name is judged by the definition of the "catch-all name" antipattern.
3. **Does the definition establish the distinction?** The definition states what the concept is, in concepts that the reader already has, precisely enough that a neighboring concept does not match it. It is neither broader nor narrower than the intended concept. The test is to check it against a case that belongs and against a similar case that does not.
4. **Is the scope visible, and do the uses follow it?** The reader can tell where the definition applies. Within that scope, replacing the term with its definition at each use leaves the meaning of each sentence unchanged. A scope beyond the document is a design decision that the user makes.

The principle can be overapplied by rejecting a needed definition because the name alone is roughly readable. A name suggests what a concept is about, but it does not determine the boundary. Another overapplication requires the cases or the neighbors used under question 3 to appear in the definition.

## Companion skills

Japanese has failure modes beyond what the preceding sections cover. The `fal-write-ja` skill addresses these failure modes. Whenever the output is Japanese, that skill applies together with this one.

Having applied these concepts while writing does not make checking the finished work redundant. Believing otherwise is itself a self-evaluation from the writer's position, which is the failure that the Reader's position principle addresses. The `fal-agentic-audit` skill supplies that separate process.
