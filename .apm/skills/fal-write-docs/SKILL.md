---
name: fal-write-docs
description: Principles for writing and editing natural-language documentation in any language, such as Markdown documents, READMEs, design docs, ADRs, and source code comments. Useful when a task involves writing or modifying a non-trivial amount of prose for future readers, including code changes that ship with substantial new documentation. Also useful where style or quality is not the stated concern. Covers what to write, where to put it, whom to write it for, how to phrase it, and which terms to use.
---

# Write Docs

Documentation here means any prose written for a future reader inside the repository: Markdown documents, READMEs, design documents, ADRs, code comments, doc comments.

LLM-written documentation fails in predictable ways, driven by four tendencies of the writer:

1. **No selection pressure.** Generating text is free, but reading and maintaining it is not. Without deliberate selection, output grows to fill the template.
2. **Context confusion.** The writing session is full of context (instructions, discussion, the previous version of the code), none of which reaches the future reader. It leaks into the output anyway.
3. **Optimizing for looking like a document.** Ritual structure, ritual phrases, and impressive-looking terms make text resemble documentation without informing anyone.
4. **Anchoring to existing structure.** When editing, the existing layout becomes the frame of reference. Structural thinking stops at the level of the immediate change, leaving higher levels unexamined.

The sections below counter these tendencies. Sections 1-7 follow the sequence of decisions made while writing:

- §1 Whether to write it at all
- §2 Where it belongs
- §3 For whom it is written
- §4 In which words
- §5 In what structure
- §6 In what paragraphs and sentences
- §7 How it fits its surroundings

Section 8 points to the Japanese-specific companion skill, and section 9 adds working discipline. A single flaw can break several sections at once; the overlap is intentional. Apply the sections while writing, and once more in a brief revision pass (§9).

## 1. Write only what earns its place

Every sentence charges the reader attention now and charges the project maintenance later. Write it only when its value to future readers beats both costs. The default is not writing.

- **Do not restate what the artifact already says.** Code, type signatures, names, and file structure speak for themselves; prose that paraphrases them adds a drift point, not information.
- **Prose earns its place by carrying what the artifact cannot express**: a contract, a non-obvious why, or the reason something exists.
- **State each fact once, in its natural home** (§2). The urge to repeat a caveat "to be safe" signals that the caveat sits in the wrong place or that sections overlap.
- **Write each claim at the scope it can honestly carry, then stop.** A pre-emptive defense (a concession or a "this does not mean..." aimed at an objection imagined while writing) plants the misreading it fears and serves the writer's anxiety, not the reader.

**Brevity comes from selection, not compression.** Cut by dropping repetition, needless detail, and sentences that do not earn their place, never by squeezing the wording of the sentences that remain. A packed phrase saves characters, not reading time. Every reader pays to unpack it.
**Minimize the staleness surface**: the set of statements that a routine change can silently falsify. Line numbers, item counts, exhaustive enumerations of specifics, copies of directory listings, version numbers, and descriptions of how current callers behave and what they pass all rot without anyone noticing. Make the point one abstraction level up, and reference code at module or function granularity. A sentence that a rename can turn into a lie is a maintenance trap.

## 2. Put each fact in its home

A correct fact in the wrong place still fails: whichever copy or location drifts first starts lying. Each fact has a natural home, determined by its lifecycle and by who looks it up.

- **A contract belongs on the thing that owns it** (the type, the function's doc comment, the module header), at the most contractual surface. Restating it downstream pays the prose cost N times and creates N drift points; reference the owner instead.
- **A home also fixes an abstraction level.** Text attached to a scope describes its subject at the level of that scope: a module header speaks of responsibilities, a function doc of its contract, a type doc of the type's role. Finer detail belongs to a smaller scope, next to what it describes. A common violation is the type doc that explains each property one by one; every one of those explanations belongs on the property's own doc. Stating detail from a higher scope is also what makes enumerations rot (§1).
- **A document set split into overview and detail has already assigned homes.** In such a set (a skill file and its references/ companion, a README and docs/, a spec and its appendices), case inventories, worked examples, and per-item explanations go to the detail layer, and the overview keeps its sections at uniform granularity. An overview section that outgrows its siblings (§7) usually holds detail that belongs in the detail layer; relocate it rather than trimming it.
- **History never belongs in documentation or comments that describe the current system.** What changed, what it replaced, and why the migration happened live in version control and documents that exist to record history, such as ADRs.
- **In code, contracts go in doc comments on the declaration**; implementation asides go in ordinary comments inside the body.
- **Orientation material belongs where a newcomer looks first**: entry points, procedures, project-wide conventions.

Before writing a fact down, ask what owns it. Write it there, and point to it from anywhere else that needs it.

## 3. Write from the reader's position

The reader is a future person, possibly yourself, who has none of the current session: no instructions, no discussion, no memory of the previous version. The reader also stands in a particular position relative to the thing described: outside its boundary, implementing it, or maintaining its internals. Identify that reader before writing (for a function, check where its callers actually live); what earns its place (§1) is judged from that reader's position. The text must be self-contained for that reader.

The rules below apply to documentation of the current system. A document whose purpose is to record a decision or event instead includes the relevant history.

- **Never write conversation-level content**: change narration ("now uses X instead"), the reason the task arose, alternatives mentioned only because you were told to avoid them, or an instruction you were given. An instruction leaks whether you cite it as provenance ("as agreed") or, more easily missed, restate its content as a fact about the subject.
- **Write in the timeless present**, as if the current design had always been the way it is. Self-check: if I had never seen the old design, would I still write this sentence?
- **The current callers are a snapshot, not properties of the thing.** Describe each thing at its own level of generality: facts about the current caller (its domain, its use case, what it passes and how it calls) do not belong in its documentation; placed there, they present themselves as properties of the thing, even when phrased as statements about the caller. Test each such sentence by restating it as a contract with the thing itself as subject, a precondition or a guarantee; keep what restates truthfully, and delete the remainder as the caller's business.
- **Mention only concepts visible from the reader's position.** A reader outside a boundary does not know the internals exist. For example, a doc comment on a public function that promises to return "the raw value, not the internal representation" contrasts the result with a type its reader has never heard of, answering a question that reader would never ask. Reach across the boundary only to explain behavior observable from outside that would otherwise surprise.
- **Reference only durable, tracked artifacts.** A pointer to a gitignored scratch file, a session plan, or "the design discussion" is a dead link from day one.

If the current state has a property that would surprise a fresh reader, state the property directly, without narrating how it came about.

**Mentions of a rejected alternative ("B, not A") deserve particular suspicion.** From inside the session they always look justifiable: A was rejected for a reason, so recording it feels like documentation. The form does have a rare legitimate use, when A is what a fresh reader would expect and the surprise of B needs addressing. But the session biases exactly that judgment; A feels expected mostly because it was just discussed. The test, applied skeptically: does a reader who has never heard of A benefit from being told about it? Usually not. Leave A out, and if the choice needs support, state the property that carries it without staging a comparison.

## 4. Use words that resolve

Every load-bearing term makes an implicit claim: this word has a meaning you can look up. A term resolves when the reader can follow it, without asking the writer, to one of three namespaces:

1. a code symbol in the repository,
2. an established term of the domain,
3. a term explicitly defined in the project's tracked documents.

The characteristic failure is the **unmarked coinage**. While organizing a topic, the writer invents a natural-looking label for a pattern it noticed, then uses the label as if it were established vocabulary. The text reads smoothly, but the term refers only to the writer's private, in-session understanding. The reader cannot follow the logic without reverse-engineering what each such term must have meant, and nothing signals that this work is required. Worse, once a coined term lands in the repository, later sessions treat it as established and spread it further.

**Bad**: "To avoid the double-write hazard, the queue performs cursor pinning." (neither term names anything in the code or the literature; each is a private label for behavior the writer had in mind)

**Good**: "To avoid writing the same record twice, the queue remembers the last processed position and resumes from it."

Typography can strengthen the false claim (backticks suggest a code symbol; quotation marks or capitalization suggest a defined term), but the failure lies in the term choice itself, not in the markup.

Rules:

- **Default to plain descriptions built from standard terms and real code symbols.** A somewhat longer but precisely-referenced phrase beats a compact invented label. This is not verbosity: a coined shortcut merely exports its cost to every reader, each of whom must reconstruct the referent alone.
- **If a concept recurs within one document often enough to deserve a name, define it at first use** ("below, this is called X"). Be restrained even then; a defined coinage is legitimate but still a burden.
- **A name that should outlive the document is a design decision.** Propose it to the user before spreading it into code or other documents; do not mint it silently.
- **Before adopting an unfamiliar term found in the repository, check that it has a definition site**: a code symbol or an explicit definition in a tracked document. A term that appears only in passing prose is not established vocabulary; it is likely a previous session's coinage. Do not amplify it. A quick check on load-bearing terms suffices; this is not a call for a full audit.

## 5. Structure has an owner

Structure exists at more than one level: how a document is sectioned, and how a document set divides into documents and files. At every level, structure serves the material, but the material of one document is not always its owner. A structure invented for this document belongs to its content; a structure shared across documents (a template, an established section convention among siblings, a genre's fixed shape, a division into files that siblings follow) belongs to the set. Identify the owner before reshaping.

- **Let content shape the structure the document owns.** Add, merge, and omit sections freely as the material demands. Padding a section so that every heading has a paragraph, or restating similar material to keep a template symmetric, is a typical failure, not thoroughness.
- **Existing structure is not fixed when editing.** When new content does not fit the current framework, redesign the structure rather than finding a local slot for the addition. Self-check: would someone writing this document from scratch, knowing all its current content, choose this structure?
- **Do not override shared structure locally.** A predictable shape across sibling documents is itself information: readers carry expectations from one document to the next, and a locally better structure breaks them for the whole set (§7 at the structural level). For example, when every module keeps its design notes in docs/design.md, moving one module's notes into its README leaves readers looking in the wrong place, however sensible the local judgment. Conforming is not padding: use the latitude the shared structure itself grants (optional sections, permitted merging). When the material does not fit even then, surface the mismatch instead of deviating silently in one document.

## 6. Paragraph and sentence discipline

- **Long-form prose is paragraphs.** Each paragraph groups closely related information and plays one clear role in the surrounding explanation, with blank lines between paragraphs. A stack of loosely joined single sentences is not a paragraph.
- **Enumerate only genuinely parallel items.** Ask first whether enumerating adds precision at all. Explanation and argument belong in paragraphs; recasting connected reasoning as bullets drops the connections and leaves fragments.
- **Notation escalates from sentence to list to table.** Few short items stay inline in a sentence. List formatting comes when inline stops being readable. A table is a last resort for short enumerable facts, with explanations in surrounding prose rather than in cells.
- **One thought per sentence.** Short declarative sentences beat chained clauses. Shortness comes from splitting thoughts, not from packing them into fewer words (§1).
- **Connectors are expensive.** An em-dash joining clauses usually marks a sentence that wants to be two. A colon fits the "heading: detail" shape; otherwise prefer separate sentences over semicolons and parentheticals.
- **Keep the register formal, precise, and neutral.** Documentation is neither marketing nor conversation.
- **Keep inline rationale comments within one or two lines.**
- **When hard-wrapping, break at clause boundaries, never mid-phrase.** Prefer starting each sentence on its own line, even when the previous line has room.

## 7. Match the surrounding corpus

A document is read alongside its neighbors, and inconsistency taxes the reader.

- **Match the density, tone, vocabulary, and language of the surrounding sections.** Before editing, check how sibling documents of the same kind handle the same thing, and align with them.
- **The nearest corpus is the document itself.** When returning to a concept the document has already phrased, reuse the earlier phrasing; a second wording for the same concept reads as a second concept.
- **Assume the first draft came out wordier than its surroundings**; LLM output runs verbose by default. After writing, compare against the neighbors and adjust.
- **Adjusting means calibrating, not minimizing.** Cut by removing sentences that do not earn their place (§1); do not squeeze the surviving sentences until they turn cryptic. Over-compression that loses the meaning fails the reader as surely as an oversized insertion that ignores its surroundings.

## 8. Japanese

Japanese has failure modes of its own beyond what the sections above cover: word choice that matches meaning but not usage, calqued idioms and rhetoric, coined kanji compounds, translationese sentence patterns. The `fal-write-ja` skill owns these rules, and it applies to all Japanese output. Apply it together with this skill whenever the text is Japanese.

## 9. Working discipline

- **Keep each edit proportional to its purpose.** When editing an existing document, leave text you are not fixing verbatim. Cosmetic churn creates diff noise and risks damaging prose that was fine.
- **After drafting, make one revision pass with fresh eyes.** The pass is part of the work, not optional polish; wording-level failures reliably surface only against a finished draft. The highest-yield checks: delete sentences that do not earn their place (§1), compare density against the surroundings (§7), and confirm that every load-bearing term resolves (§4).
- **Surface problems that prose cannot fix.** When writing exposes code structure that needs an apology or a concept that needs a real name, report it to the user instead of papering over it.
