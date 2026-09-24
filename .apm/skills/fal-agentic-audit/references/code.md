# Cues and judgment notes for code and comments

This file holds the cues and the judgment notes for code structure, code comments, and doc comments. The `fal-agentic-audit` skill states how to use them.

## Cues

A cue marks where to suspect an antipattern. Each cue ends by naming the antipatterns to suspect. The `fal-agentic-vocabulary` skill decides whether the suspicion holds. The material for that judgment is the definition of the antipattern and the rest of the section that holds it, including the principle's test and the paragraph on its overapplication. Where the antipattern has an entry under "Judgment notes" in this file, that entry is part of the material. Where a cue says to do something, such as to read, to search, or to compare, that work is part of finding the cue.

### Comment phrasing

- Negation or comparison phrasing, such as "not X," "rather than," or "instead of." Suspect "unsolicited clarification," "session leak," and "unsolicited history."
- A comment that defends a claim or a choice against a doubt or an objection, such as `// This is intentional` or `// Not a bug`. Suspect "unsolicited clarification" and "session leak."
- A caveat or a concession beside a claim. Suspect "unsolicited clarification."
- A comment stating a prohibition. Suspect "unsolicited clarification" and "context-bound statement."
- A comment stating what the code does not handle. Suspect "unsolicited clarification" and "context-bound statement."
- Narration of what the change replaced, such as `// renamed from Z` or `// previously returned Y`. Suspect "unsolicited history."
- A qualifier that refers to a previous version, such as "the refactored path." Suspect "version-bound description."
- A comment that paraphrases the name, the signature, or the code beside it. Suspect "redundant statement."
- A comment that tells the reader how to read the code rather than why the code is as it is. Suspect "wrong-layer patch."
- A comment that names the current callers or what they are assumed to have done, such as "Used by X" or "Assumes callers validated the input." Suspect "caller-bound framing" and "snapshot reasoning."
- A doc comment that describes the thing in the caller's domain, use case, or terminology. Suspect "caller-bound framing."
- A justification that cites who currently calls the thing, which inputs occur, or the present shape or placement of the code. Suspect "snapshot reasoning."
- Wording that claims sameness, such as "same as X" or "as in X." Suspect "false analogy."
- A comment on an optional field stating that the value is present under some condition, such as "should never be null when X." Suspect "wrong-layer patch."
- A doc comment on a public surface naming types, mechanisms, or steps that its reader cannot see. Suspect "exposed internals" and "unabstracted detail."
- A doc comment on a declaration that details one of its members, such as a type doc describing the behavior of one field. Suspect "unabstracted detail."
- A connector that joins clauses without stating how they relate, such as an em dash, a semicolon, or "which also." Suspect "split focus" and "unstated relation."
- A verb of physical action with an abstract noun as its subject or object. Suspect "culture-bound phrase."
- An inanimate subject given will, speech, or feeling. Suspect "culture-bound phrase."
- An idiom of general English. Suspect "culture-bound phrase."
- Comments in one region drawing words from one figurative system. Suspect "culture-bound phrase."

### Enumerations, references, and terms in comments

- A list or an enumeration, in list notation or inline, such as "A, B, or C" or "A, B, and C." Ask what the surrounding text says it lists, whether other plausible items could also belong, and whether the wording presents a complete set or examples. Suspect "unabstracted detail," "staleness surface," and "unstated relation."
- A parenthesis after a term, such as `X (Y)`, whose relation to the term is not stated. Suspect "unstated relation."
- A defined name in parentheses without the kind of thing that it names. Suspect "unstated relation."
- An example placed beside the rule that it illustrates, with no label marking it as an example. Suspect "unstated relation."
- A concrete value or identifier, such as a count, a path, a line number, a version, or a date. Suspect "staleness surface."
- A reference that locates its target by position, such as "the call above." Suspect "unclear reference" and "staleness surface."
- A demonstrative, a pronoun, or a noun phrase presented as already known, such as "the items," standing in place of a name. Suspect "unclear reference."
- A point stated again, in the same or in different wording. Suspect "redundant statement" and "unclear reference."
- A reference that neither the repository nor a public source resolves, such as "as discussed" or "as agreed." Suspect "vestige" and "session leak."
- A term in backticks that names no symbol. Search the repository first. Suspect "unmarked coinage."
- A compound label that leads to no declaration. Search the repository first. Suspect "unmarked coinage."
- A phrase that has to be reread to determine how its words relate. Suspect "packed phrase."
- A bare general word for a qualified concept, such as "budget" for a retry budget. Suspect "stripped term."

### Names

- A qualifier that refers to a previous version, such as `newParser`, `parserV2`, or `legacyHandler`. Suspect "version-bound description" and "needless backward compatibility."
- An identifier that has to be reread to determine how its words relate, such as one stacking three nouns and a qualifier. Suspect "packed phrase."
- An identifier that is a bare general word, such as a `budget` field that holds a retry budget or a type named `Handler`. Suspect "stripped term," and suspect "catch-all name" as a case that the Structural naming principle governs.
- Several similar names in one region. Suspect "wrong-layer patch," as the first of the causes that the Structural naming principle lists.
- A qualifier that only tells two names apart. Suspect "wrong-layer patch," as the first of the causes that the Structural naming principle lists.
- A symbol named for its caller. Suspect "caller-bound framing," as the second of the causes that the Structural naming principle lists.
- A public symbol named for its representation or mechanism. Suspect "exposed internals," as the second of the causes that the Structural naming principle lists.
- A name that needs "and." Suspect "split focus."

### Structure

- A function or a block too large to understand in one reading. Suspect "overlong block."
- A body that mixes orchestration with low-level manipulation. Suspect "split focus."
- A caller that reaches into another module's internals. Suspect "misplacement."
- A flag or a special case that the change in hand inserted into the existing structure. Suspect "under-scoped change."
- A change that only adds, leaving every existing element in place. Suspect "under-scoped change."
- Entry points or wrappers multiplied per use site. Suspect "under-scoped change."
- The old interface kept beside its replacement, such as an alias, a deprecated wrapper, a parameter kept so that callers do not break, or both paths behind a flag. Suspect "needless backward compatibility."
- A null check or a guard. Suspect "vestige" and "wrong-layer patch."
- A branch that returns a default, substitutes a null or an empty value, or ignores an error. Suspect "silent fallback."

### Units questioned or compared

A word or a form does not mark these cues. Each is found by examining the units that the cue names, as the cue states.

- A departure from the form shared by siblings, such as in naming convention, argument order, layout, or language. Read the siblings first. Suspect "sibling mismatch."
- A change applied to one unit whose siblings keep the old form. Suspect "sibling mismatch" and "under-scoped change."
- Explanation or code devoted to one case, such as a comment, a helper, a parameter, or a branch, where neighboring cases receive less or none. Compare what each case received with the region as a whole. Suspect "salience leak."
- An element or a claim that nothing in scope requires or supports, such as a helper, a type, or a parameter that nothing uses. Ask what in scope motivates it. Suspect "session leak" and "vestige."
- A rule or a verdict stated without limit or conditions. Ask whether a reader can recover its scope from the code and comment, taking any stated reason into account. Suspect "context-bound statement."
- A comment whose reading is not obvious, where neither the code nor the nearby comments settle how it was meant. Suspect "context-bound statement."
- A function or a block. Ask what subject its logic is about, judging by the data that it reads and writes, and whether the module that holds it covers that subject. Suspect "misplacement."
- A code element, such as a constant or a branch, that repeats a definition from elsewhere. Suspect "staleness surface" and "misplacement."
- A constraint that the code enforces, such as a validation or an assertion. Ask which party guarantees or imposes it, and whether the enforcement sits with that party. Suspect "misplacement" and "under-scoped change."
- A unit of code, such as a function, a block, a type, or a module. Ask what its parts do and how they relate to one another. Suspect "split focus" and "misplacement."
- A new code element, such as a function, type, or module, that overlaps an existing element in responsibility or in the cases that it enumerates. Search beyond the module under edit. Suspect "unintegrated addition" and "under-scoped change."
- A specialization of an existing general type defined as though unrelated. Suspect "unintegrated addition."
- A construct that also appears in code that looks similar, such as a guard, a lock, a retry, or a cache. Suspect "false analogy."
- A name at its declaration. Ask whether a reader who has not seen its use sites can tell what it names. Suspect "caller-bound framing" and "wrong-layer patch."
- A comment that states what the code does or guarantees. Read the code and verify each claim. Suspect "silent contradiction."
- Every comment in focus, and every part of one. Suspect "over-documentation."

## Judgment notes

The `fal-agentic-vocabulary` skill decides whether a suspected antipattern holds and what the fix is. The notes here cover only what that skill does not settle. Examples are what to do when the judgment cannot be made, a fix that the auditor might not think of, a weighting that the definition does not carry, and a case that this medium handles in its own way.

### Redundant statement

A short doc summary that reads as redundant with the name passes where it states the intent or the scope that the name leaves open.

### Unsolicited clarification

A negation or a prohibition written as its own sentence earns its place only where its benefit to the reader is substantial. Where deleting a qualifier would make the claim false, the qualifier stays, or the claim is reworded to the scope that it can carry.

### Staleness surface

Where code that repeats a definition from elsewhere has to stay, the fix can make a change at the source either reach this place or fail visibly. An example of the first is deriving the value from the source. An example of the second is an exhaustive match that stops compiling when a case is added.

### Needless backward compatibility

Where the request did not keep an old interface that sits beside its replacement, the fix replaces it and migrates the callers. Where it cannot be determined whether the request kept it, leave the interface and report it.

### False analogy

Where the reason behind what is established is not known, such as the reason that a copied construct had at its source, report the construct or the wording and leave the code untouched.

### Silent contradiction

If the code might be wrong instead of the comment, report the conflict and leave both sides untouched.

### Caller-bound framing

A bare caller reference such as "Used by X" can remain while the structure work that removes it is deferred. Elaborating on how the caller behaves or what it passes turns a caller fact into an intrinsic property and crosses the boundary.

### Session leak

Where the element or the comment marks a property of the code that surprises a fresh reader, the fix can replace it with a direct statement of the property instead of deleting it.

### Salience leak

Where one case has dedicated code that neighboring cases do not have, the case can sometimes be absorbed into the general one.

### Context-bound statement

Where the context in which the comment was written is not known, leave the comment untouched, because a rewording replaces the writer's meaning with the auditor's guess. Report the comment and the meaning taken from the code and the comment, for someone who holds the context to compare.

### Version-bound description

Where removing the qualifier leaves nothing that distinguishes the thing, two elements compete for one name, and the design has not decided between them. The fix is that decision. The superseded element is usually a vestige to delete.

### Unmarked coinage

A concept that keeps needing a coined label usually wants a declaration of its own. Judge the concept under the Structural naming principle before replacing the label.

### Unclear reference

Each reference carries some risk of a failed lookup, so a passage with many references warrants testing each one.

### Packed phrase

Where a name does not unpack to a readable length, the concept that it names is suspect before the name is. Judge the concept under the Structural naming principle before renaming.

### Culture-bound phrase

An inanimate subject passes where the wording is established technical usage, such as a function, a type, or a rule taking a verb of stating or requiring. So does a phrase that resolves as a code symbol of the repository.
