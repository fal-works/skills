---
name: fal-agentic-audit
description: Audit and fix a finished change against the fal-agentic-vocabulary concepts, covering documentation, comments, and code structure.
---

# Agentic audit

This skill is an audit-and-fix pass over a finished change: code structure, code comments, doc comments, and Markdown documents.

The `fal-agentic-vocabulary` skill defines the concepts: the named principles and the antipatterns. Read that skill first. This skill assumes its definitions and adds two things: the cues by which an antipattern shows in a finished artifact, and judgment notes that cover what the definitions do not settle, such as what to do when the judgment cannot be made.

The cues and the judgment notes live in [code and comments](./references/code.md) and [documents](./references/documents.md), one file per medium. Each file lists the cues by what the auditor observes, and each cue names the antipatterns to suspect.

## Set up a fresh context

By default, the auditor stands where the writer stood rather than where the reader will stand. The prose in scope therefore deserves stricter judgment than a first impression suggests. When practical, run the audit in a new session or subagent whose input carries only this skill, the `fal-agentic-vocabulary` skill, and the files in scope.

The fresh context still needs the change itself: which files and regions it touched, and what it did to them. Step 1 reads the focus of the pass from that description. A diff serves. The reasons, the discussion, the rejected alternatives, and the wording of the instructions do not belong in that description. Passing them rebuilds the position that the fresh context exists to leave.

Some antipatterns concern the session, such as "session leak" and "salience leak." Only an auditor who holds the session can recognize an instruction restated or the case that the session raised. A fresh auditor holds a counterpart of each: the question of why the artifact says this at all. It arises at a reference that nothing resolves, an element that nothing in scope motivates, and emphasis that the material itself does not explain. Neither position sees every leak, so someone who held the session reviews what the fresh pass reports.

Whoever held the session is subject to the defensive bias toward keeping the work as it was written. For them, a reported finding is a lead, as a cue is for the auditor in step 3. A finding whose explanation is wrong can still mark a real problem. A reason for rejecting a finding is itself a claim, and it is verified before the finding is rejected.

## Run the audit

### 1. Set the scope and the focus

Scope starts with the files that a just-finished change touched or the files that the user names. It includes related material needed to judge the work. Preexisting text is in scope, not only the text that the current change touched.

Within that scope, the regions do not carry equal risk. A change in hand puts the risk in the changed text and in the seams where it meets what was already there. A reading of the change alone misses the seam risk. A defect that the user points to is the starting point of the pass and not its scope, because text that shows one antipattern usually shows others. Take its neighbors with it. Without a change or a defect that the user points to, this step sets no focus. The end-to-end read of step 2 shows where to look.

### 2. Read each file end-to-end

Placement and duplication are invisible line by line.

Work file by file. Where a judgment depends on a unit's relationship to other units, read those units before making the judgment.

### 3. Examine the work and judge by the definitions

Read the reference file that the scope calls for. When the change touched both media, read both reference files.

Use the vocabulary's principles to decide what to examine in the work and its relationships. Form questions and comparisons from those principles, including where no listed cue applies.

Cues help locate possible failures and identify antipatterns to suspect. Consult them as relevant features appear, in no fixed order. Also carry out the questions and comparisons that the cues call for, because reading alone does not reveal every cue.

Judge each suspected failure against the vocabulary's definitions and the relevant judgment notes. A cue alone does not establish a finding. The available evidence sometimes cannot settle the judgment. Concerns can also conflict over the same text, so judge which concern governs the case.

Unless the "Authority and reporting" section limits the pass to reporting, fix what the judgment confirms. Fix structure before judging the names and the explanations that depend on it, because a structural fix invalidates a judgment made on the old structure.

### 4. Revise the audit's own edits

The edits are themselves a change and carry the failures that a change carries, so audit what the audit wrote. The scope of this pass is the audit's own edits and what they invalidate, not the files again. A deletion writes nothing, but it makes the text before it and the text after it adjacent. Read both again from the reader's position.

Where a fix changed structure, text that already passed under the old structure has to be judged again, and the new structure can show new cues. A name that the old structure explains and a description of the part that moved are examples.

One failure is the audit's own doing: preserving the content while keeping the edit small packs an existing unit instead of rebuilding it. This failure is the "under-scoped change" antipattern with the audit's own edit as the change.

## Authority and reporting

This skill fixes the problems that it finds. It deletes, trims, moves, and rewrites on its own judgment. When the request excludes edits, such as by asking only for a report, a list, or an evaluation, limit the pass to reporting. The words "audit" and "review" alone do not exclude edits.

Report the findings, whether the pass fixed them or proposes the fix. For each finding, state its location, the observed problem, and the fix made or proposed. Where a vocabulary concept applies, explain why it applies. Also state how the pass was narrowed in scope, because a pass that narrows silently reads as one that covered everything.

## Editing discipline

- Keep each edit proportional to its purpose, and leave everything that is not being fixed verbatim: same wording, same punctuation, same line breaks, same code. Churn creates diff noise and risks damaging what was fine.
- What is bounded is the reach of the pass, not the size of a fix. Where a fix calls for the unit to be rebuilt, rebuild it.
- Because of the additive bias, the auditor tends to fix a problem by adding text, which is often not the best fix. Before adding text, consider whether rewriting the existing text fixes the problem.
- Because of the defensive bias, the auditor avoids judging a passage unnecessary. The auditor can also make that judgment and then rewrite the passage instead of removing it. Where the audit finds a passage unnecessary, remove it.
- Before a replacement becomes final, set it beside the original: everything the original stated survives unless a finding calls for the change. A split counts as a replacement, with its pieces read in place of the original. The usual losses are a dropped qualifier and a dropped relation between clauses. A dropped qualifier leaves a noun phrase stating a broader claim than the original did.
- A replacement keeps the original's notation unless a finding calls for the change.
- A restructure preserves behavior unless a finding calls for the change, as a "silent fallback" finding does.
- A comment-only edit is not always inert. Type-bearing dialects, such as JSDoc types and Python type comments, feed static analysis, and a Markdown edit can break links and anchors. Check what the edit could have broken.

## Japanese

Japanese prose has failure modes beyond what this skill covers. The `fal-write-ja` skill owns the target state, and the `fal-improve-ja` skill holds the audit procedure and examples. When the prose under audit is Japanese, apply them together with this skill.
