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

The fix is a redesign of the affected scope, not a better insertion. Sketch it before editing.

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

Test each kept interface: did the request keep it, or only the writer's caution? Keep what the request kept. Replace the rest, and migrate the callers.

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

Move each to its owner. A rule that the module guarantees belongs in its own types, and a rule that one use site imposes belongs in that use site's layer.

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

Even work that exists to question the current shape of the code can rest its judgments on that shape, so it is not exempt. The cue is placement, classification, or existence justified by any of the following:

- Who currently calls the thing
- What they pass
- Which inputs occur
- The code's present shape

Test: would the judgment hold if the callers changed? Rederive the judgment from what the thing itself guarantees, or move the constraint to the use site that imposes it.

## Over-documentation

This check has no cue, and every comment in focus is a candidate. A comment that does not earn its place reads like one that does, so nothing on the surface separates them.

Test by removal: take the comment out and name what the reader of the code then lacks. An answer names a contract or a why that the code does not show. That the comment is accurate is not an answer.

Delete what has no answer. Where the answer covers only part of a comment, keep that part and drop the rest.

## Redundant statement

The cue is a comment that paraphrases the function name, the type signature, or the code next to it.

Delete, or rewrite at the level of what callers can rely on.

A short doc summary that reads as redundant with the name passes where it states the intent or the scope that the name leaves open.

## Unsolicited justification

The cue is a comment answering an objection that nobody raised, such as `// This is intentional` or `// Not a bug: we need this because...`.

Test by deletion: take the comment out and name the surprise left unexplained. A comment that answers one is a why-comment and passes. Delete the defense that leaves none.

Placement decides the case. The same "because..." is essential where the property would surprise a fresh reader, and unnecessary where the reader takes the property for granted.

## Staleness surface

In a comment, a routine change elsewhere can falsify any of the following without touching the comment's file:

- An enumeration of current callers
- A count of cases handled
- A list of the fields that a function reads

In code, the cue is a constant or a branch enumerating what another module currently defines.

Restate one abstraction level up, or delete when the code is clear without it.

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

A line-by-line pass will not catch it, so compare what each part received against the region as a whole. Adjust the emphasis, or absorb the special case back into the general one.

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

Restate the description as a contract with the thing as its subject. For a name, inspect the structure before renaming, because the name is usually carrying a distinction that the design does not express.

The bare caller reference is a borderline case, not a clean pass. "Used by X" and "Assumes callers validated the input" are honest about an asymmetry, and the reference stays within the boundary. Keep it, and read it as a signal that structure work was deferred. Where the thing is meant to serve only that one caller, the boundary is what needs fixing rather than the description. Elaborating on how X behaves or what it passes crosses the boundary and turns a caller-specific fact into an intrinsic property.

## Sibling mismatch

Any of the following is a cue:

- Naming convention, argument order, or layout departing from the sibling set
- Linked copies and callers left unvisited by a change

Read the siblings before judging one member. Conform, or report the mismatch when the material genuinely does not fit.

## Overlong block

The cue is a function or block too large for easy comprehension.

Ask the content question first: does everything in it earn its place? Split only afterward. Splitting content that should have been dropped is wasted work.

## Split focus

In code, any of the following is a cue:

- Orchestration and low-level manipulation in one body
- A type that has accumulated a second responsibility
- A name that needs "and"

In comment prose, the cue is a connector joining clauses that each carry their own thought, such as an em dash, a semicolon, or "which also."

Split along the roles. Where the current structure gives them no clean seam, recompose the region instead of cutting the body where it stands.

## Unmarked coinage

Any of the following is a cue:

- A comment using a term in backticks that names no symbol
- A compound label that the reader cannot follow to any declaration

Search the repository before judging: a term appearing only in passing comments is likely a previous session's coinage, not established vocabulary. Replace it with a plain description or the real symbol, and do not propagate the coined term. A concept that keeps needing a coined label usually wants a declaration of its own.

## Catch-all name

The cue is a general-purpose word as the name of a type, function, field, or module, such as "layer," "element," "component," "manager," or "handler."

Test: does the name distinguish this concept from its neighbors? Treat the name as a symptom first. It often indicates a contract or an abstraction level that the design has not fixed, and renaming then hides the cause. The Structural naming principle governs this case.

## Stripped term

The cue is an identifier that drops the qualifier of the concept that it names. Examples are a `budget` field that holds a retry budget and a `table` parameter that takes a staging table. The same shortening can appear in a comment.

Restore the full term at every surface that a reader meets without the enclosing scope in view. A local variable inside that scope can carry less.

## Packed phrase

The cue is a name or a comment phrase whose words have to be reread to determine how they relate. An identifier stacking three nouns and a qualifier is the usual form.

Unpack it. A name that will not unpack to a readable length is naming more than one thing.
