---
name: fal-agentic-audit
description: Audit and fix a finished change against the fal-agentic-vocabulary concepts, covering documentation, comments, and code structure.
---

# Agentic audit

This skill is an audit-and-fix pass over a finished change: code structure, code comments, doc comments, and Markdown documents.

The `fal-agentic-vocabulary` skill defines the concepts: the named principles and the antipatterns. Read that skill first. This skill assumes its definitions and adds two things: the cues that mark where to suspect an antipattern or where to apply a principle in a finished artifact, and judgment notes that cover what the definitions do not settle, such as what to do when the judgment cannot be made.

The cues and the judgment notes are in [code and comments](./references/code.md) and [documents](./references/documents.md), one file per medium. Each file lists the cues by what the auditor observes, and each cue names the antipatterns to suspect or the principles to apply.

## Set up a fresh context

By default, the auditor has the writer's knowledge rather than the reader's. The prose in scope therefore deserves stricter judgment than a first impression suggests. When practical, run the audit in a new session or subagent whose input contains only this skill, the `fal-agentic-vocabulary` skill, and the files in scope.

The fresh context still needs the change itself: which files and regions it touched, and what it did to them. Step 1 determines the focus of the pass from that description. A diff is sufficient. The reasons, the discussion, the rejected alternatives, and the wording of the instructions do not belong in that description. Passing them restores the writer's knowledge that the fresh context exists to exclude.

Some antipatterns concern the session, such as "session leak" and "salience leak." Only an auditor who has access to the session context can recognize an instruction restated or the case that the session raised. A fresh auditor has a counterpart of each: the question of why the artifact says this at all. It arises at a reference that nothing resolves, an element that nothing in scope motivates, and emphasis that the material itself does not explain. Neither auditor detects every leak, so someone who has access to the session context reviews what the fresh pass reports.

That reviewer is subject to the defensive bias toward keeping the work as it was written. For them, a reported finding is a hypothesis to check, as a cue is for the auditor in step 3. A finding whose explanation is wrong can still indicate a real problem. A reason for rejecting a finding is itself a claim, and it is verified before the finding is rejected.

## Run the audit

### 1. Set the scope and the focus

Scope starts with the files that a just-finished change touched or the files that the user names. It includes related material needed to judge the work. Preexisting text is in scope, not only the text that the current change touched.

Within that scope, the regions do not have equal risk. Where there is a change to audit, the risk is in the changed text and at the boundaries where it adjoins the existing text. A reading of the change alone misses the risk at those boundaries. A defect that the user points to is the starting point of the pass and not its scope, because text that shows one antipattern usually shows others. Include the text around it in the scope. Without a change or a defect that the user points to, this step sets no focus. The end-to-end read of step 2 shows where to look.

### 2. Read each file end-to-end

Placement and duplication are judged by comparing a unit with related units, and those units can be anywhere in the file.

Work file by file. Where a judgment depends on a unit's relationship to other units, read those units before making the judgment.

### 3. Examine the work and judge by the definitions

Read the reference file that the scope requires. When the change touched both media, read both reference files.

Use the vocabulary's principles to decide what to examine in the work and its relationships. Form questions and comparisons from those principles, including where no listed cue applies.

Cues help locate possible failures and identify the antipatterns to suspect or the principles to apply. Consult them as relevant features appear, in no fixed order. Also ask the questions and make the comparisons that the cues require, because reading alone does not reveal every cue.

Judge each suspected failure against the vocabulary's definitions and the relevant judgment notes. A cue alone does not establish a finding. The available evidence sometimes cannot settle the judgment. Concerns can also conflict over the same text, so judge which concern takes precedence in the case.

Unless the "Authority and reporting" section limits the pass to reporting, fix what the judgment confirms. Fix structure before judging the names and the explanations that depend on it, because a structural fix invalidates a judgment made on the old structure.

### 4. Revise the audit's own edits

The edits are themselves a change and can have the failures that any change can have, so audit what the audit wrote. The scope of this pass is the audit's own edits and what they invalidate, not the files again. A deletion writes nothing, but it makes the text before it and the text after it adjacent. Read both again from the reader's position.

Where a fix changed structure, text that already passed under the old structure has to be judged again, and the new structure can show new cues. A name that the old structure explains and a description of the part that moved are examples.

One failure is caused by the audit itself: preserving the content while keeping the edit small condenses an existing unit instead of rebuilding it. This failure is the "under-scoped change" antipattern with the audit's own edit as the change.

## Authority and reporting

This skill covers both auditing and fixing. When the request excludes edits, such as by asking only for a report, a list, or an evaluation, limit the pass to reporting. Otherwise, fix the confirmed problems by deleting, shortening, moving, or rewriting as the judgment requires. The words "audit" and "review" alone do not exclude edits.

Report the findings, whether the pass fixed them or proposes the fix. For each finding, state its location, the observed problem, and the fix made or proposed. Where a vocabulary concept applies, explain why it applies. Also state how the pass was narrowed in scope, because a pass whose scope was narrowed without a statement reads as one that covered everything.

## Editing discipline

- Keep each edit proportional to its purpose, and leave everything that is not being fixed verbatim: same wording, same punctuation, same line breaks, same code. Unnecessary changes make the diff harder to read and risk damaging text that was correct.
- What is limited is the scope of the pass, not the size of a fix. Where a fix requires the unit to be rebuilt, rebuild it.
- Because of the additive bias, the auditor tends to fix a problem by adding text, which is often not the best fix. Before adding text, consider whether rewriting the existing text fixes the problem.
- Because of the defensive bias, the auditor avoids judging a passage unnecessary. The auditor can also make that judgment and then rewrite the passage instead of removing it. Where the audit finds a passage unnecessary, remove it.
- Before a replacement becomes final, compare it with the original: everything the original stated is kept unless a finding requires the change. A split counts as a replacement, with its pieces read in place of the original. The usual losses are an omitted qualifier and an omitted relation between clauses. An omitted qualifier leaves a noun phrase stating a broader claim than the original did.
- A replacement keeps the original's notation unless a finding requires the change.
- A restructure preserves behavior unless a finding requires the change, as a "silent fallback" finding does.
- A comment-only edit does not always leave behavior unchanged. Type-bearing dialects, such as JSDoc types and Python type comments, are read by static analysis, and a Markdown edit can break links and anchors. Check what the edit could have broken.

## Japanese

Japanese prose has failure modes beyond what this skill covers. The `fal-write-ja` skill defines the target state, and the `fal-improve-ja` skill contains the audit procedure and examples. When the prose under audit is Japanese, apply them together with this skill.
