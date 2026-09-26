# Cues and judgment notes for documents

This file holds the cues and the judgment notes for Markdown documents and other prose written for future readers. The `fal-agentic-audit` skill states how to use them.

## Cues

A cue marks where to suspect an antipattern or where to apply a principle. Each cue ends by naming the antipatterns to suspect or the principles to apply. The `fal-agentic-vocabulary` skill decides whether a suspected failure holds. The material for that judgment is the section that holds the antipattern or the principle, including the principle's test and the paragraph on its overapplication. Where the antipattern or the principle has an entry under "Judgment notes" in this file, that entry is part of the material. Where a cue says to do something, such as to read, to search, or to compare, that work is part of finding the cue.

### Phrasing

- Negation or comparison phrasing, such as "not X," "rather than," "instead of," or "this does not mean." Suspect "unsolicited clarification," "session leak," and "unsolicited history."
- A passage that defends a claim or a choice against a doubt or an objection, such as "this is intentional." Suspect "unsolicited clarification" and "session leak."
- A caveat, a concession, or an edge-case note beside a claim. Suspect "unsolicited clarification."
- A prohibition. Suspect "unsolicited clarification" and "context-bound statement."
- A statement of what is out of scope. Suspect "unsolicited clarification" and "context-bound statement."
- Narration of what the change replaced, such as "previously," "renamed from Z," or "now uses X instead of Y." Suspect "unsolicited history."
- A qualifier that depends on the old version, such as "the new pipeline," "the refactored module," or "the current approach." Suspect "version-bound description."
- An opening line that echoes its heading. Suspect "redundant statement."
- A passage that explains how to read the document rather than explaining its subject. Suspect "wrong-layer patch."
- A justification that cites the current state of the subject or of the document's outline as its reason. Suspect "snapshot reasoning" and "unwarranted condition."
- A condition stated for adopting or rejecting something, such as "only where," "unless," or "whenever X, Y is unnecessary." Suspect "unwarranted condition."
- Wording that claims sameness, such as "the same as," "likewise," or "a kind of." Suspect "false analogy."
- A conclusion that cites a rule or an earlier decision without stating why it was established. Suspect "false analogy."
- A connector that joins clauses without stating how they relate, such as an em dash, a semicolon, or "which also." Suspect "split focus" and "unstated relation."
- A verb whose primary sense describes a body or a physical object, such as its action, posture, dwelling, or movement, with an abstract or inanimate noun as its subject or object, such as information that "sits" in a file. Suspect "culture-bound phrase."
- An inanimate subject given will, speech, feeling, or cognition, such as a cache that "remembers" a value or a module that "knows" its callers. Suspect "culture-bound phrase."
- An idiom of general English. Suspect "culture-bound phrase."
- Successive sentences drawing words from one figurative system. Suspect "culture-bound phrase."

### Enumerations, references, and terms

- A list or an enumeration, in list notation or inline, such as "A, B, or C" or "A, B, and C." Ask what the surrounding text says it lists, whether other plausible items could also belong, and whether the wording presents a complete set or examples. Suspect "unabstracted detail," "staleness surface," and "unstated relation."
- A parenthesis after a term, such as `X (Y)`, whose relation to the term is not stated. Suspect "unstated relation."
- A defined name in parentheses without the kind of thing that it names. Suspect "unstated relation."
- A condition, an exception, or a limit that a claim depends on, placed in parentheses, such as "returns X (when the cache is enabled)." Suspect "unstated relation."
- An example sentence in body prose with no label marking it as an example. Suspect "unstated relation."
- A concrete value or identifier, such as a count, a path, a line number, a version, or a date. Suspect "staleness surface."
- A reference that locates its target by position, such as "the items above." Suspect "unclear reference" and "staleness surface."
- A demonstrative, a pronoun, or a noun phrase presented as already known, such as "the items," standing in place of a name. Suspect "unclear reference."
- A point stated again, in the same or in different wording. Suspect "redundant statement" and "unclear reference."
- A fact stated at the place that covers its subject and again elsewhere. Suspect "redundant statement" and "misplacement."
- A reference that neither the repository nor a public source resolves, such as "as discussed," "as agreed," or a cross-reference into a document that has since been reorganized. Suspect "vestige" and "session leak."
- A term in backticks that the repository neither declares nor defines. Search the repository first. Suspect "unmarked coinage."
- A compound label that the repository does not define. Search the repository first. Suspect "unmarked coinage."
- A phrase that has to be reread to determine how its words relate. Suspect "packed phrase."
- A general-purpose word serving as the document's fixed term for one concept, such as "layer," "element," or "component." Suspect "catch-all name."
- A passage that states what a term means, such as an entry in a terms section or a sentence of the form "below, this is called X." Apply the Defined terms principle.
- A defined term appearing without its qualifier. Suspect "stripped term."
- A second term for a concept that the document has already named. Suspect "sibling mismatch" and "vestige."

### Structure

- A paragraph or a list item of roughly 500 or more characters of Latin-script text, or of fewer characters in a denser script. Suspect "overlong block."
- A paragraph that changes role midway. Suspect "split focus."
- A list whose items are of different kinds. Suspect "split focus."
- A heading that needs "and" to cover its body. Suspect "split focus."
- A document serving two roles, such as a tutorial that also serves as a reference. Suspect "split focus."
- A document holding material of another genre, such as a README with changelog entries. Suspect "misplacement."
- Worked examples or per-item explanation in an overview document that has a detail layer. Suspect "misplacement" and "unabstracted detail."
- A section that grew by clauses or sentences added one at a time, such as a spec with one clause per requirement. Suspect "under-scoped change."
- A document that is addressed to readers outside a boundary and names mechanisms or types that those readers cannot see. Suspect "exposed internals."
- A shared subject described in one consumer's vocabulary. Suspect "caller-bound framing."
- A general capability described as the procedure of one use case. Suspect "caller-bound framing."

### Units questioned or compared

A word or a form does not mark these cues. Each is found by examining the units that the cue names, as the cue states.

- A section that runs longer than its sibling sections. Read the siblings first. Suspect "unabstracted detail," "sibling mismatch," and "salience leak."
- A list item several times the length of the items around it. Suspect "sibling mismatch."
- Density, tone, vocabulary, language, or structure departing from the sibling sections or documents. Read the siblings first. Suspect "sibling mismatch."
- A change applied to one unit whose siblings keep the old form. Suspect "sibling mismatch" and "under-scoped change."
- A point given more room, more emphasis, or a narrower example than its place in the section warrants. An exception given more room than its rule is one form. Compare what each point received with the section as a whole. Suspect "salience leak."
- A claim that nothing in scope supports, such as the content of an instruction restated as a fact. Ask what in scope motivates it. Suspect "session leak."
- A claim about how a reader or a process will behave, or about what a fix or a safeguard will prevent. Ask whether its grounds support it as firmly as it is stated, and, where it is inferred, whether the text says so. Apply the Examined assumptions and Words that resolve principles.
- A rule, a decision, or a verdict stated without limit or conditions. Ask whether a reader can recover its scope from the document, taking any stated reason into account. Suspect "context-bound statement."
- A statement whose reading is not obvious, where nothing nearby settles how it was meant. Suspect "context-bound statement."
- A fact in a section. Ask what the fact is about, and whether the heading of the section covers that subject. Suspect "misplacement" and "under-scoped change."
- A section and its heading. Ask whether the content would have been written if the heading did not call for it. Suspect "wrong-layer patch."
- A section that overlaps what another document states. Search beyond the document under edit. Suspect "unintegrated addition."
- A classification or a vocabulary parallel to the one in use. Search beyond the document under edit. Suspect "unintegrated addition."
- A section that describes a design. Compare what it describes with the current design. Suspect "vestige."
- A term for a component or a concept of the subject. Compare it with the name that the current code or design gives to the same thing. Suspect "vestige."
- A criterion applied to an artifact. Compare that artifact with the one for which the criterion was set. Suspect "false analogy."
- A claim about a thing, such as what it states, contains, or covers, whether that thing is adjacent or elsewhere. Find the thing and verify each claim. Verify an absence claim across its full scope. Suspect "silent contradiction."
- Every passage in focus, and every part of one. Suspect "over-documentation."

## Judgment notes

The `fal-agentic-vocabulary` skill decides whether a suspected failure holds and what the fix is. The notes here cover only what that skill does not settle. Examples are what to do when the judgment cannot be made, a fix that the auditor might not think of, a weighting that the definition does not carry, and a case that this medium handles in its own way.

### Redundant statement

An opening line that reads as an echo of its heading passes where it states the scope that the heading leaves open. Where a copy is deleted and a reader at its place still needs the fact, the fix leaves a reference to the place that keeps it.

### Unsolicited clarification

A negation or a prohibition written as its own sentence earns its place only where its benefit to the reader is substantial. Where deleting a qualifier would make the claim false, the qualifier stays, or the claim is reworded to the scope that it can carry.

### Wrong-layer patch

When a shared template defines the structure, a section written to fill one of its headings is conforming.

### False analogy

Where the reason behind what is established is not known, report the passage and leave it untouched.

### Silent contradiction

If the described thing might be wrong instead of the description, report the conflict and leave both sides untouched.

### Session leak

Where the passage marks a property of the subject that surprises a fresh reader, the fix can replace it with a direct statement of the property instead of deleting it.

### Context-bound statement

Where the context in which the statement was made is not known, leave the statement untouched, because a rewording replaces the writer's meaning with the auditor's guess. Report the statement and the meaning taken from the document, for someone who holds the context to compare.

### Unclear reference

Each reference carries some risk of a failed lookup, so a passage with many references warrants testing each one.

### Catch-all name

The replacement term is used everywhere that the document names the concept.

### Culture-bound phrase

For a phrase that a cue marks as a suspected culture-bound phrase, state the operation, state, or relation that the phrase means without reusing its wording. Then compare that statement with the original. Where the statement names the operation, state, or relation more directly and keeps the meaning, it replaces the original, as "is written in" replaces "sits in." Frequent use in English technical prose is not a reason to keep the original. Where the statement is no more direct, as for a document that "states" a rule, the original stays.

### Principle of Defined terms

Judging that a definition is too broad or too narrow needs a ground for the intended boundary, such as a specification, the purpose of the document, or a consistent use of the term in the document. Where no ground settles the boundary, report the definition and leave it untouched. Where a use departs from the definition and it is unclear which one is wrong, report both and leave them untouched.
