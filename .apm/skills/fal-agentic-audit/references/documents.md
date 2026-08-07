# Checks for documents

These are the checks for Markdown documents and other prose written for future readers. The `fal-agentic-audit` skill states how to run them.

## Under-scoped change

Any of the following is an addition placed where the old outline allowed insertion rather than where the content belongs:

- A spec that accumulated clauses one requirement at a time
- A description modified sentence by sentence as behavior changed

The fix is reshaping the affected sections, not a better insertion. Sketch it before editing.

## Vestige

Any of the following is a cue:

- A section still describing a retired architecture
- A cross-reference into a reorganized document
- Terminology retained from a previous version's vocabulary

Update to the current design, or delete.

## Unintegrated addition

Any of the following is a cue:

- A new section overlapping what another document already states
- A classification or vocabulary parallel to the one in use

Relate the addition to the existing structure, or merge it.

Sections that model different concepts might look duplicated and still need to diverge freely. The check aims at the addition that was never judged.

## Wrong home

Any of the following is a cue:

- A fact in a section whose subject does not own it
- Detail below the scope's level
- An overview section that runs longer than its siblings because it holds material belonging to the detail layer

A fact stated in only one place can still be in the wrong one. The check runs at document scale too. A document that has departed from its genre holds material whose home is another document. A README that has accumulated changelog entries is the usual form.

Move the fact to the scope that owns it.

## Wrong-layer patch

The cue is a long explanation compensating for structure. It tells the reader how to read the thing rather than why the thing is the way it is.

Padding is the other form: a section written only because the heading exists.

Do not settle for refining the compensating text. Sketch the restructure that would make the prose unnecessary. Delete the padding or revise the headings.

When a shared template owns the structure, filling its sections is conforming, not padding.

## Snapshot reasoning

Even work that exists to question the current state can rest its claims on that state, so it is not exempt. Any of the following is a cue:

- A new claim grounded in what an existing document already says
- A classification or a boundary kept because the current outline has it
- A name chosen because the existing text already uses it
- The subject's present state offered as the reason to keep the current description

Test: does the cited text own the fact, or is it only a place where the fact currently appears? Rederive the claim from its subject, or cite the owner instead.

A spec, a contract, a decision record, and a defined term own what they state, and resting on them is not this failure. Matching the form that the siblings share is what the "sibling mismatch" check asks for, not this failure. Resting a new claim on what a sibling asserts is this failure.

## Over-documentation

This check has no cue, and every passage in focus is a candidate. Prose that does not earn its place reads like prose that does, so nothing on the surface separates them.

Test by removal: take the passage out and name what the reader then lacks. An answer names something that the subject does not show on its own. That the passage is true, relevant, or well written is not an answer.

Delete what has no answer. Where the answer covers only part of a passage, keep that part and drop the rest.

## Redundant statement

Any of the following is a cue:

- The same fact at its owner and again downstream
- Earlier prose of the same document rephrased

Keep the statement at its owner, delete the copies, and reference the owner where a pointer is needed.

An opening line that reads as an echo of its heading passes where it states the scope that the heading leaves open.

## Unsolicited justification

Any of the following is a cue when attached to a claim that is accurate without it:

- A concession
- An edge-case note
- A "this does not mean..."

Test by deletion: if the claim reads as meant without it, then delete the defense. If removing it leaves a likely misreading standing, then the caveat passes. If the claim turns false, then the qualifier is load-bearing: keep it, or reword the claim to the scope that the claim can honestly carry.

Placement decides the case. The same caveat earns its place beside a claim that a reader is likely to misread, and not beside one that the surrounding text has already bounded.

## Staleness surface

Any of the following is a cue:

- A line count
- A version number
- An exhaustive enumeration of specifics
- A description of what current callers pass
- A closed list representing an open set

Restate one abstraction level up, or delete when the document is clear without it.

## Silent contradiction

The description disagrees with the thing that it describes. Unlike a comment's code, the described thing is not adjacent: find it and read it.

If the description is stale, then fix or delete it. If the thing might be wrong instead, then report the conflict and leave both sides untouched.

## Session leak

Any of the following is a cue:

- A reference that neither the repository nor a public source resolves
- Comparison and negation phrasing, such as "not X," "instead of," or "rather than"
- The content of an instruction restated as a fact about the subject, with no marker like "as agreed"

Test each claim: would someone who never sat in the session still write it? Delete what fails. If the current state has a surprising property, then state the property directly.

Comparison phrasing also marks legitimate least-surprise documentation. Keep the alternative only when a fresh reader would expect it, and state why the choice was made.

## Salience leak

No phrasing marks this failure. The cue is disproportion, and any of the following is one:

- An exception given more room than its rule
- An example narrower than the point that it illustrates

A sentence-by-sentence pass will not catch it, so compare each point's emphasis to the section as a whole. Adjust the emphasis or remove the excess.

## History leak

The cue is a qualifier that depends on the old version, such as "the new pipeline," "the refactored module," or "the current approach."

Remove the qualifier. If what remains is unclear, then the description itself is the problem.

A document whose purpose is to record a decision or event is outside this check.

## Unsolicited history

The cue is narration of what the change replaced, such as "now uses X instead of Y," "renamed from Z," or "previously."

Delete the narration and keep the description of the current state.

A document whose purpose is to record a decision or event is outside this check.

## Exposed internals

The cue is a document addressed to readers outside a boundary, naming mechanisms or types that those readers cannot see.

Rewrite in terms visible from the reader's position.

An internal fact deliberately fixed as a promise passes: stating it is the commitment.

## Caller-bound framing

Any of the following is a cue:

- A document describing a shared subject in one consumer's vocabulary
- A general capability described as the procedure of one use case

Restate the description with the shared subject as its subject, in terms that every reader of the document can follow.

## Sibling mismatch

Any of the following is a cue:

- Density, tone, vocabulary, language, or structure departing from sibling sections and documents
- A second term for a concept that the document has already named

Skim the siblings before editing one document. Mismatch is a corpus-level symptom that a single-file read cannot catch. Calibrate toward the corpus.

Calibration is not minimization.

## Overlong block

The cue is size: roughly 500 characters of Latin-script text in a paragraph or list item, and fewer in denser scripts.

Judge each suspect by role, not by the trigger. The content question comes first: does everything in it earn its place? A split follows only when the focus has genuinely split.

## Split focus

In a sentence, the cue is a connector joining clauses that each carry their own thought: an em dash, a semicolon, or "which also." In a paragraph, the cue is a role change midway.

Split at the role boundary. Where no existing break falls on it, recompose the passage instead of cutting at the nearest seam.

A colon that introduces detail after a label passes.

## Unmarked coinage

Any of the following is a cue:

- A natural-looking compound label with no definition site
- Capitalization suggesting a defined term that no document defines

Search the repository for a definition site before judging: a term appearing only in passing prose is likely a previous session's coinage, not established vocabulary. Replace it with a plain description or the real symbol, and do not propagate the coined term. Where the concept genuinely recurs in the document, define it at first use instead.

## Catch-all name

The cue is a general-purpose word serving as a document's fixed term for one specific concept, such as "layer," "element," or "component."

Test: does the term distinguish this concept from its neighbors? Replace it with a word specific enough to name that concept and no other. Use the replacement everywhere that the document names the concept.

## Stripped term

The cue is a defined term appearing without its qualifier at a later mention, such as "retry budget" shortened to "budget" or "staging table" to "table."

Restore the full term wherever the surrounding text has not already fixed the referent.

## Packed phrase

The cue is having to reread a phrase to determine how its words relate.

Unpack into a longer phrase that lets the reader recover the relationship.
