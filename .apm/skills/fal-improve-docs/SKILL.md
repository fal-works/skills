---
name: fal-improve-docs
description: Audit and fix existing documentation and source code comments against the fal-write-docs principles. Applies to Markdown documents, READMEs, design docs, ADRs, and comments in any language or doc-comment dialect. Useful after implementing or reviewing a change that adds or modifies a substantial amount of prose or comments, and when the user asks to clean up, review, or audit comments or documentation. Companion to the fal-write-docs skill, which defines the target state.
---

# Improve Docs

This skill is an audit-and-fix pass over existing prose: code comments, doc comments, and Markdown documents. The target state is defined by the fal-write-docs skill; read its SKILL.md before auditing. The checks below reference its sections (§N means a fal-write-docs section) and use vocabulary defined there. What this skill adds is what auditing needs beyond the norms: how violations look in existing text, what fix each one takes, and when a finding is bigger than prose.

The goal of a pass: every surviving sentence earns its place, and nothing the reader needs is missing. Default to subtraction; removing dead weight makes the survivors louder. But the pass is not only subtractive: move density toward the places that need it, and add the whys that are missing.

## Auditor's stance

Existing prose deserves less charity than it invites. A leaked-context comment reads as plausible during audit precisely because the writer did have reasons; those reasons do not transfer to a future reader, and an auditor who imputes them on the writer's behalf keeps dead weight. When text cites an external referent (a spec section, a sibling doc, a prior agreement), verify the referent is reachable from the repository. An unverifiable reference is leaked context by definition (§3). The audit itself runs from a compromised position: the auditor reads with the internals fully in view, exactly where boundary leaks (check 5) look natural, and replacement prose written mid-audit is as exposed to them as the original. When practical, run the audit in a fresh context: a new session or subagent that reads only this skill, fal-write-docs, and the files in scope. An auditor that never saw the writing session inherits none of its leaked context, and reads from the position the future reader will occupy.

At the same time, do not overshoot. These survive the audit:

- Short doc summaries that look name-redundant but state intent or scope.
- Implementation details that callers can rely on. "Binary search" in a doc tells callers O(log n); that is contract, not mechanism (§1).
- Explicit caller-relationship framing ("Used by X", "Assumes callers validated the input"). Naming the reference or the obligation is honest about an asymmetry; keep it, and read it as a soft signal that structure work was deferred (check 13). The survivor is the bare reference: details of how X behaves or what it passes are check 5 material.
- Rationale placed where the property would surprise a fresh reader. The same "because..." is load-bearing there, and noise where the reader takes the property for granted.

## The checks

Scope: when triggered by a change, the files it touched; when invoked explicitly, what the user named. Read each file in scope end-to-end before editing anything, because placement and duplication problems are invisible line-by-line. Pre-existing text is in scope, not only what the current change touched; text from the current change deserves extra skepticism.

**Information failures**

1. **The artifact already says it.** Prose paraphrases the implementation, the type, or the name. Delete, or rewrite at the level of what callers can rely on (§1).
2. **It lies.** The text says X; the artifact does Y. Usually drift. If the text is stale, fix or delete it. If the code may be wrong instead, that is a bug: report the conflict and leave both sides untouched, including any adjacent prose asserting the same claim. This check has no fal-write-docs counterpart; only an audit catches drift.
3. **The why is missing.** Non-obvious code (a workaround, a subtle invariant, surprising behavior) with no explanation. Add a brief why-comment, one or two lines (§1, §6).

**Context leaks**

4. **The writing session leaked.** Change history, instruction provenance ("as agreed", "per spec §3.2"), references to untracked files, mentions of rejected alternatives. Delete; if the current state has a surprising property, state the property directly (§3). Comparison and negation phrasing ("not X", "instead of", "rather than") is the cue to pause: it marks either this leak or legitimate least-surprise documentation. Apply §3's test, and keep the alternative only when a fresh reader would expect it, saying why the choice was made.
5. **The wrong side of the boundary leaked.** In one direction, the current caller's domain, use case, or behavior stated in the thing's own doc; restate it as a contract with the thing as subject, or delete it (§3). In the other, internal concepts addressed to a reader outside the boundary, who cannot see them and would never ask about them; rewrite in terms visible from the reader's position (§3). If the thing should only ever serve that one caller, that is a structure question, not a doc fix (check 13).

**Placement**

6. **Wrong home.** The same fact stated at its owner and restated downstream, or type behavior repeated at every call site. Hoist to the most contractual surface and drop the copies (§2).
7. **Wrong altitude.** Text enumerates identifiers, fields, branches, or cases where one sentence at its scope's level carries the content. Move up one level; anything item-specific goes next to the item (§1, §2).

**Words**

8. **A term does not resolve.** A load-bearing term that names nothing in the code, the domain, or the project's defined vocabulary. Likely a previous session's coinage; replace it with a plain description or the real symbol, and do not let it spread (§4).

**Form**

9. **Ritual and padding.** Meta-narration, throat-clearing introductions, summary closings, sections filled to keep a template symmetric. Mostly a Markdown symptom. Delete (§5).
10. **Pre-emptive defense.** A concession, an edge-case note, or a "this does not mean..." qualifying a claim that is accurate without it. It reads as precision, which is why it survives; its only audience is an objection the writer imagined while writing. Test by deletion: if the claim stays accurate, delete the defense. If it becomes false, the qualification is load-bearing and passes the audit; keep it, or reword the claim to the scope it can honestly carry when that reads cleaner (§1).
11. **Verbose construction.** One sentence doing the work of three, chained with connectors. Split into short declarative sentences (§6).
12. **Corpus mismatch.** Density, tone, vocabulary, or language diverging from the surrounding sections and sibling documents. Calibrate toward the corpus; calibration is not minimization (§7).

**Escalation**

13. **The prose apologizes for the structure.** A long explanation whose real job is to compensate for poorly factored code or a tangled document; typically it explains how to read the thing rather than why the thing is the way it is. This is the most valuable finding an audit can produce, and the temptation is to polish the apology and move on. Do not settle for that: sketch the restructure (naming, extraction, reshaping) that would make the prose unnecessary, and include it in your report. Whether to apply it yourself is an authority question; see below.

## Authority

Make prose edits on your own judgment: delete, trim, move, and rewrite without asking first, then report what you changed and why, grouped by action. Two findings stay outside unilateral action:

- **A lie over possibly-buggy code (check 2).** Taking either side silently corrupts something. Report it.
- **Restructuring code or documents (check 13).** When the audit is a finishing pass on code work you are already doing, apply the restructure; it is part of the work. When you were asked specifically to audit comments or docs, do not expand into code edits on your own; deliver the audit and propose the restructure concretely.

## Editing discipline

- Prefer, in order: delete, trim, move, rewrite. Deletion is the highest-leverage edit; rewriting is the easiest to get wrong.
- Text that passes the audit stays verbatim: same wording, same punctuation, same line breaks. Cosmetic churn creates diff noise and can turn a survivor into a casualty on the next pass.
- Replacement prose follows fal-write-docs in full, especially sentence discipline (§6) and corpus fit (§7). Do not remove one long sentence and write another.

## Verify

- Comment edits: run the project's lint and type checks. Type-bearing dialects (JSDoc types, Python type comments, Sphinx directives) can change static-analysis results even in a "comment-only" edit.
- Markdown edits: check the links and anchors you touched; build the docs if the project has a docs build.

## Target notes

**Comments.** The lie check (2) requires reading the annotated code, not skimming it; verify each claim against what the code does. Contracts live in the doc comment on the declaration; implementation asides live in ordinary comments inside the body (§2).

**Markdown.** Before editing one document, skim its siblings: duplication (check 6) and mismatch (check 12) are corpus-level symptoms that a single-file read cannot catch. Confirm the document still serves its genre; a README that has drifted into a changelog is a placement problem (§2).

**Japanese.** Japanese prose has failure modes beyond what the checks above cover: word choice that matches meaning but not usage, calqued idioms, coined kanji compounds, English punctuation habits. The fal-japanese skill owns the target state for these; when the prose under audit is Japanese, read it and audit against its rules as well.
