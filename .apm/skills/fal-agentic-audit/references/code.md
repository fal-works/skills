# Checks for code and comments

These are the checks for code structure, code comments, and doc comments. The `fal-agentic-audit` skill states how to run them.

## Under-scoped change

In the edit, any of the following is a cue:

- A flag or special case inserted throughout the old structure
- A change that leaves every existing element in place

In the result, any of the following is a cue:

- Entry points multiplied per use site
- A new function beside one with the same responsibility
- A new type enumerating nearly the same cases as an existing one
- A general module given a constraint that only one use site imposes

The fix is a redesign of the affected scope, not a better insertion.

## Vestige

Any of the following is a cue:

- A helper that has no caller left
- A branch for a state that the current design excludes
- A guard for an assumption no longer in force

Test against the current design, not the old one: does it give the element a reason to exist? Delete what fails the test.

## Needless backward compatibility

Any of the following keeps the old interface alongside its replacement:

- A kept alias
- A deprecated wrapper nobody requested
- A parameter preserved "so callers do not break"
- Both code paths behind a flag

Test each kept interface: did the request keep it, or only the writer's caution? Keep what the request kept. Replace the rest, and migrate the callers. Where it cannot be determined which, leave the interface and report it.

## Unintegrated addition

Any of the following is a cue:

- A new type, function, or module overlapping what already exists
- A specialization of an existing general type defined as though unrelated

Relate the addition: reuse, extend, or replace the existing element.

Types and functions that model different concepts might look duplicated and still need to diverge freely. The check aims at the addition that was never judged.

## Wrong home

Any of the following is a cue:

- A caller accessing another module's internals
- Logic placed away from its subject
- A constraint enforced far from the party that guarantees it
- An enumeration in a comment, whether written as a list or as "A, B, or C"
- A constant or a branch enumerating what another module defines

An enumeration in a comment is suspected of being detail below the level of the thing that the comment documents. The question is whether that level calls for the items themselves, or only for what they have in common.

Move each to the place that covers its subject. A rule that the module guarantees belongs in its own types, and a rule that one use site imposes belongs in that use site's layer. Restate an enumeration at the level of the place where it sits. What the restatement drops is often unnecessary, and what is still needed belongs next to the item that it describes.

## Wrong-layer patch

In code, any of the following is a cue:

- A run-time check for a state that the types could exclude
- An optional field annotated "should never be null when X"

In a comment, the cue is an explanation that tells the reader how to read the code rather than why the code is that way.

In naming, any of the following is a cue:

- Several similar names concentrated in one region
- A qualifier added only to tell two names apart
- A name that resolves only through its use site

Sketch the fix one structural level up instead of refining the workaround. Each naming cue asks whether the design expresses the distinction that the name is carrying. Where it does not, a rename is the symptom-layer fix.

## Snapshot reasoning

The cue is placement, classification, or existence justified by any of the following:

- Who currently calls the thing
- What they pass
- Which inputs occur
- The code's present shape

Test: why can the cited state be taken as given? That it is the current state is not an answer. Where no reason holds, rederive the judgment from what the thing itself guarantees, or move the constraint to the use site that imposes it.

## False analogy

The cue is a construct, such as a lock, a guard, or a retry, copied from code that looks similar.

Test: does the reason that the construct had at its source hold at the destination? Remove the construct where the reason does not hold. Where the reason is not known, report the construct and leave the code untouched.

## Over-documentation

This check has no cue, and every comment in focus is a candidate.

Test by removal: take the comment out and name what the reader of the code then lacks. An answer names a contract or a why that the code does not show. That the comment is accurate is not an answer.

Delete what has no answer. Where the answer covers only part of a comment, keep that part and drop the rest.

## Redundant statement

The cue is a comment that paraphrases the function name, the type signature, or the code next to it.

Delete, or rewrite at the level of what callers can rely on.

A short doc summary that reads as redundant with the name passes where it states the intent or the scope that the name leaves open.

## Unsolicited clarification

Any of the following is a cue:

- A comment answering an objection that nobody raised, such as `// This is intentional` or `// Not a bug: we need this because...`
- A comment negating an alternative that nothing in the code suggests, such as `// no retry here`
- A comment stating what the code does not handle, where nothing in the code suggests that it would
- A comment that only forbids the opposite of what a neighboring comment requires

Test by deletion: take the comment out and name the question that the reader would then have, such as a surprise left unexplained or a contract that the code does not show. A comment that answers one passes. Delete the clarification that answers none.

A negation or prohibition written as its own sentence earns its place only when its benefit to the reader is substantial enough to justify it.

## Staleness surface

In a comment, the cue is an enumeration, whether written as a list or as "A, B, or C." A routine change elsewhere can falsify it without touching the comment's file.

In code, the cue is a constant or a branch that repeats what another module defines. A change to that module does not reach it.

For a comment, restate one abstraction level up, or delete when the code is clear without it. For code, the fixes vary with the case, and each makes a change at the source reach this place or fail visibly.

## Silent contradiction

The comment says X. The code does Y. Detection requires reading the annotated code, not skimming it: verify each claim.

If the text is stale, then fix or delete it. If the code might be wrong instead, then that is a bug: report the conflict and leave both sides untouched.

## Silent fallback

Any of the following is a cue:

- A default returned on input that must never occur
- A caught-and-ignored error
- A substituted null or empty value

If the invariant must hold, then assert the violation instead of hiding it.

## Session leak

Any of the following is a cue:

- A comment citing a file or an agreement that neither the repository nor a public source resolves
- Negation phrasing in a comment, such as "not X" or "rather than"
- The content of an instruction restated as a fact about the code
- An element that exists only because the session raised it, such as a guard for a case that nothing in scope requires

Test each claim and each element: would someone who never sat in the session write it? Delete what fails. If the code has a surprising property, then state the property directly.

A comment naming the rejected alternative earns its place only where a fresh reader would expect that alternative. It also has to say why the choice was made.

## Salience leak

No phrasing marks this failure. The cue is disproportion, and any of the following is one:

- A comment on the case that the session raised and none on its neighbors
- A helper extracted for that one case
- A parameter or branch covering only the dimension that was discussed

Compare what each part received against the region as a whole. Adjust the emphasis, or absorb the special case back into the general one.

## Context-bound statement

No phrasing marks this failure, and the code alone seldom shows whether the failure is present. Any of the following is a cue:

- A comment stating a constraint or a prohibition without limit, where the reader cannot recover its reason or the scope that was meant
- A comment stating a verdict as a general fact, where neither the code nor the comment states the conditions
- A comment whose reading is not obvious, where neither the code nor the nearby comments settle how it was meant

Where the context in which the comment was written is known, reword the comment or delete it.

Where that context is not known, leave the comment untouched, because a rewording would replace the writer's meaning with the auditor's guess. Report the comment and the meaning taken from the code and the comment, for someone who holds the context to compare.

## History leak

The cue is an identifier carrying a qualifier that means something only against the old version, such as `newParser`, `parserV2`, or `legacyHandler`. The same qualifier can appear in a comment, such as "the refactored path."

Remove the qualifier and name the thing by what it is. If nothing distinguishing remains, then two elements are competing for one name, and the design has not decided between them. The fix is that decision, and the superseded element is usually a vestige to delete.

## Unsolicited history

The cue is a comment narrating what the change replaced, such as `// renamed from Z` or `// previously returned Y`.

Delete the narration and keep the description of the current state. Version control holds the history.

## Exposed internals

Any of the following is a cue:

- A doc comment on a public surface naming types, mechanisms, or steps that its reader cannot see
- A public symbol named for its internal representation or mechanism

Rewrite in terms visible from outside, or delete. Rename a symbol by what it promises.

A mechanism deliberately fixed as a promise passes. "Binary search" in a doc commits the function to O(log n), and naming it is the commitment.

## Caller-bound framing

Any of the following is a cue:

- A symbol named for the caller that happens to use it
- A doc comment describing the thing in the caller's domain, use case, or terminology

Restate the description as a contract with the thing as its subject. For a name, inspect the structure before renaming.

The bare caller reference is a borderline case, not a clean pass. "Used by X" and "Assumes callers validated the input" strictly fall under this antipattern. Such a comment may remain because the refactoring is unfinished or is not worth its cost, so it need not be fixed at once. Keep it, and read it as a signal that structure work was deferred. Where the thing is meant to serve only that one caller, the boundary is what needs fixing rather than the description. Elaborating on how X behaves or what it passes crosses the boundary and turns a caller-specific fact into an intrinsic property.

## Sibling mismatch

Any of the following is a cue:

- Naming convention, argument order, or layout departing from the sibling set
- A change applied to one unit while its siblings keep the old form

Read the siblings before judging one member. Conform, or report the mismatch when the material genuinely does not fit.

## Overlong block

The cue is a function or block too large for easy comprehension.

Ask the content question first: does everything in it earn its place? Split only afterward. Splitting content that should have been dropped is wasted work.

## Split focus

In code, any of the following is a cue:

- Orchestration and low-level manipulation in one body
- A type that has accumulated a second responsibility
- A module that can be described only by listing its contents
- A name that needs "and"

In comment prose, the cue is a connector joining clauses that each carry their own thought, such as an em dash, a semicolon, or "which also." A connective that itself states how the clauses relate, such as "because" or "but," is not this cue: what it states is content. A split made there anyway is complete only when the resulting sentences still state the relation.

Split along the roles. Where the current structure gives them no clean seam, recompose the region instead of cutting the body where it stands.

## Unmarked coinage

Any of the following is a cue:

- A comment using a term in backticks that names no symbol
- A compound label that the reader cannot follow to any declaration

Search the repository before judging. A term that appears in comments, and that the repository neither declares nor defines, is likely a previous session's coinage, not established vocabulary. Replace it with a plain description or the real symbol, and do not propagate the coined term. A concept that keeps needing a coined label usually wants a declaration of its own.

## Catch-all name

The cue is a general-purpose word as the name of a type, function, field, or module, such as "layer," "element," "component," "manager," or "handler."

Test: does the name distinguish this concept from its neighbors? Treat the name as a symptom first, a case that the Structural naming principle governs.

## Stripped term

The cue is an identifier that drops the qualifier of the concept that it names. Examples are a `budget` field that holds a retry budget and a `table` parameter that takes a staging table. The same shortening can appear in a comment.

Restore the full term at every surface that a reader meets without the enclosing scope in view. A local variable inside that scope can carry less.

## Packed phrase

The cue is a name or a comment phrase whose words have to be reread to determine how they relate. An identifier stacking three nouns and a qualifier is the usual form.

Unpack it. A name that will not unpack to a readable length is naming more than one thing.

## Unstated relation

In a comment, any of the following is a cue:

- A parenthesis after a term with no stated relation between the two
- A defined name in parentheses without the kind of thing that it names
- An example placed beside the rule that it illustrates, with no label marking it as an example
- An enumeration that does not show whether it lists every member or only some
- A list where the text that introduces it does not state what it enumerates

State the relation in words, or label the status, marking a partial enumeration as examples.

## Assumed connection

In a comment, any of the following is a cue:

- A demonstrative or a pronoun used in place of a name
- A noun phrase presented as already known, such as "the items"
- A reference that locates its target by its place in the file, such as "the call above"
- A point stated again in wording that differs from the comment that stated it first

Test each reference by naming the target that the reader would reach. It fails where the target is far back, or where more than one candidate fits it. Each reference carries some risk of a failed lookup, so many references in one passage increase that risk.

Name the target in place of the reference. Where a point returns, repeat the wording that first stated it.

## Culture-bound phrase

In a comment, any of the following is a cue:

- A verb of physical action with an abstract noun as its subject or object
- An inanimate subject given will, speech, or feeling
- An idiom of general English
- Comments in one region using words from one figurative system

A term of figurative origin that the domain has established, such as "thread" or "pipeline," resolves as an established term of the domain and passes. So does a phrase that resolves as a code symbol of the repository. A subject such as a function, a type, or a rule taking a verb of stating or requiring is established technical usage and also passes. Replace the rest with the direct statement of the operation or the fact.
