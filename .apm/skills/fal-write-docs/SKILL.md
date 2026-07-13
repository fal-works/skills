---
name: fal-write-docs
description: Principles for writing and editing natural-language documentation in any language, such as Markdown documents, READMEs, design docs, ADRs, and source code comments. Useful when a task involves writing or modifying a non-trivial amount of prose for future readers, including code changes that ship with substantial new documentation. Also useful where style or quality is not the stated concern. Covers what to write, where to put it, how to phrase it, and which terms to use.
---

# Write Docs

Documentation here means any prose written for a future reader inside the repository: Markdown documents, READMEs, design documents, ADRs, code comments, doc comments.

LLM-written documentation fails in predictable ways, driven by three tendencies of the writer:

1. **No selection pressure.** Generating text is free, but reading and maintaining it is not. Without deliberate selection, output grows to fill the template.
2. **Context confusion.** The writing session is full of context (instructions, discussion, the previous version of the code), none of which reaches the future reader. It leaks into the output anyway.
3. **Optimizing for looking like a document.** Ritual structure, ritual phrases, and impressive-looking terms make text resemble documentation without informing anyone.

The sections below counter these tendencies. Sections 1-7 follow the sequence of decisions made while writing:

- §1 Whether to write it at all
- §2 Where it belongs
- §3 For whom it is written
- §4 In which words
- §5 In what structure
- §6 In what sentences
- §7 How it fits its surroundings

Section 8 points to the Japanese-specific companion skill, and section 9 adds working discipline. A single flaw can break several sections at once; the overlap is intentional. Apply the sections while writing, and once more in a brief revision pass (§9).

## 1. Write only what earns its place

Every sentence charges the reader attention now and charges the project maintenance later. Write it only when its value to future readers beats both costs. The default is not writing.

- Do not restate what the artifact already says. Code, type signatures, names, and file structure speak for themselves; prose that paraphrases them adds a drift point, not information.
- Prose earns its place by carrying what the artifact cannot express: a contract, a non-obvious why, or the reason something exists.
- State each fact once, in its natural home (§2). The urge to repeat a caveat "to be safe" signals that the caveat sits in the wrong place or that sections overlap.
- Write each claim at the scope it can honestly carry, then stop. A pre-emptive defense (a concession or a "this does not mean..." aimed at an objection imagined while writing) plants the misreading it fears and serves the writer's anxiety, not the reader.

Brevity comes from selection, not compression. Cut by dropping repetition, needless detail, and sentences that do not earn their place, never by squeezing the wording of the sentences that remain. A packed phrase saves characters, not reading time. Every reader pays to unpack it.
Minimize the staleness surface: the set of statements that a routine change can silently falsify. Line numbers, item counts, exhaustive enumerations of specifics, copies of directory listings, and version numbers all rot without anyone noticing. Make the point one abstraction level up, and reference code at module or function granularity. A sentence that a rename can turn into a lie is a maintenance trap.

## 2. Put each fact in its home

A correct fact in the wrong place still fails: whichever copy or location drifts first starts lying. Each fact has a natural home, determined by its lifecycle and by who looks it up.

- A contract belongs on the thing that owns it (the type, the function's doc comment, the module header), at the most contractual surface. Restating it downstream pays the prose cost N times and creates N drift points; reference the owner instead.
- A home also fixes an abstraction level. Text attached to a scope describes its subject at the level of that scope: a module header speaks of responsibilities, a function doc of its contract, a type doc of the type's role. Finer detail belongs to a smaller scope, next to what it describes. A common violation is the type doc that explains each property one by one; every one of those explanations belongs on the property's own doc. Stating detail from a higher scope is also what makes enumerations rot (§1).
- A document set that separates an overview from a detail layer (a skill file and its references/ companion, a README and docs/, a spec and its appendices) has already assigned homes: case inventories, worked examples, and per-item explanations go to the detail layer, and the overview keeps its sections at uniform granularity. An overview section that outgrows its siblings (§7) usually holds detail that belongs in the detail layer; relocate it rather than trimming it.
- History belongs in version control and ADRs. What changed, what it replaced, and why the migration happened never belong in a document body or a comment.
- In code, contracts go in doc comments on the declaration; implementation asides go in ordinary comments inside the body.
- Orientation material (entry points, procedures, project-wide conventions) belongs where a newcomer looks first.

Before writing a fact down, ask what owns it. Write it there, and point to it from anywhere else that needs it.

## 3. Write for a reader with zero session context

The reader is a future person, possibly yourself, who has none of the current session: no instructions, no discussion, no memory of the previous version. The text must be self-contained for that reader.

- Never write conversation-level content: change narration ("now uses X instead"), instruction provenance ("as agreed", "per the spec discussion"), the reason the task arose, or alternatives mentioned only because you were told to avoid them.
- Write in the timeless present, as if the current design had always been the way it is. Self-check: if I had never seen the old design, would I still write this sentence?
- Describe each thing at its own level of generality. The current caller's domain or use case must not pose as a property of the thing itself.
- Reference only durable, tracked artifacts. A pointer to a gitignored scratch file, a session plan, or "the design discussion" is a dead link from day one.

If the current state has a property that would surprise a fresh reader, state the property directly, without narrating how it came about.

Mentions of a rejected alternative ("B, not A") deserve particular suspicion. From inside the session they always look justifiable: A was rejected for a reason, so recording it feels like documentation. The form does have a rare legitimate use, when A is what a fresh reader would expect and the surprise of B needs addressing. But the session biases exactly that judgment; A feels expected mostly because it was just discussed. The test, applied skeptically: does a reader who has never heard of A benefit from being told about it? Usually not. Leave A out, and if the choice needs support, state the property that carries it without staging a comparison.

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

- Default to plain descriptions built from standard terms and real code symbols. A somewhat longer but precisely-referenced phrase beats a compact invented label. This is not verbosity: a coined shortcut merely exports its cost to every reader, each of whom must reconstruct the referent alone.
- If a concept recurs within one document often enough to deserve a name, define it at first use ("below, this is called X"). Be restrained even then; a defined coinage is legitimate but still a burden.
- If a name should outlive the document and spread into code or other documents, that is a naming decision, which is design work. Propose it to the user; do not mint it silently.
- Before adopting an unfamiliar term found in the repository, check that it has a definition site: a code symbol or an explicit definition in a tracked document. A term that appears only in passing prose is not established vocabulary; it is likely a previous session's coinage. Do not amplify it. A quick check on load-bearing terms suffices; this is not a call for a full audit.

## 5. Let content shape structure

Structure serves the material, never the reverse.

- Add, merge, and omit sections freely as the material demands. Padding a section so that every heading has a paragraph, or restating similar material to keep a template symmetric, is a typical failure, not thoroughness.
- Existing structure is not fixed when editing. When new content does not fit the current framework, redesign the structure rather than finding a local slot for the addition. Self-check: would someone writing this document from scratch, knowing all its current content, choose this structure?
- Long-form prose is paragraphs. Each paragraph groups closely related information and plays one clear role in the surrounding explanation, with blank lines between paragraphs. A stack of loosely joined single sentences is not a paragraph.
- Treat enumeration as an escalation ladder. First ask whether enumerating adds precision at all; if the items are few and short, keep them inline in a sentence; only then reach for list formatting. Tables are a last resort for short enumerable facts, with explanations in surrounding prose rather than in cells.
- Skip ritual: meta-narration ("This document describes..."), throat-clearing introductions, and summary closings ("In summary...", 「まとめ」). When the content ends, the document ends.

## 6. Sentence discipline

- One thought per sentence. Short declarative sentences beat chained clauses. Shortness comes from splitting thoughts, not from packing them into fewer words (§1).
- Connectors are expensive. An em-dash joining clauses usually marks a sentence that wants to be two. A colon fits the "heading: detail" shape; otherwise prefer separate sentences over semicolons and parentheticals.
- Keep the register formal, precise, and neutral. Documentation is neither marketing nor conversation.
- Keep inline rationale comments within one or two lines.
- When hard-wrapping, break at clause boundaries, never mid-phrase. Prefer starting each sentence on its own line, even when the previous line has room.

## 7. Match the surrounding corpus

A document is read alongside its neighbors, and inconsistency taxes the reader.

- When extending an existing document, match the density, tone, vocabulary, and language of the surrounding sections. Before editing, check how sibling documents of the same kind handle the same thing, and align with them.
- Assume the first draft came out wordier than its surroundings; LLM output runs verbose by default. After writing, compare against the neighbors and adjust.
- Adjusting means calibrating, not minimizing. Cut by removing sentences that do not earn their place (§1); do not squeeze the surviving sentences until they turn cryptic. Over-compression that loses the meaning fails the reader as surely as an oversized insertion that ignores its surroundings.

## 8. Japanese

Japanese has failure modes of its own beyond what the sections above cover: word choice that matches meaning but not usage, calqued idioms and rhetoric, coined kanji compounds, translationese sentence patterns. The `fal-japanese` skill owns these rules, and it applies to all Japanese output. Apply it together with this skill whenever the text is Japanese.

## 9. Working discipline

- Keep each edit proportional to its purpose. When editing an existing document, leave text you are not fixing verbatim. Cosmetic churn creates diff noise and risks damaging prose that was fine.
- After drafting, make one revision pass with fresh eyes. The highest-yield checks: delete sentences that do not earn their place (§1), compare density against the surroundings (§7), and confirm that every load-bearing term resolves (§4).
- When writing exposes a problem that prose cannot fix, such as code structure that needs an apology or a concept that needs a real name, surface it to the user instead of papering over it.
