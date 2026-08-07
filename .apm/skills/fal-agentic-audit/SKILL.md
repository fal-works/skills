---
name: fal-agentic-audit
description: Audit and fix a finished change against the fal-agentic-vocabulary concepts, covering documentation, comments, and code structure.
---

# Agentic audit

This skill is an audit-and-fix pass over a finished change: code structure, code comments, doc comments, and Markdown documents.

The `fal-agentic-vocabulary` skill defines the concepts: the named principles and the antipatterns. Read that skill first. The checks assume its definitions. Each check adds two things: the cues by which its failure shows in a finished artifact, and the fix.

The checks live in [code and comments](./references/code.md) and [documents](./references/documents.md), one file per medium. Each check is named for the antipattern that it detects.

## Set up a fresh context

By default, the auditor stands where the writer stood rather than where the reader will stand. Existing prose therefore deserves stricter judgment than a first impression suggests. When practical, run the audit in a new session or subagent whose input carries only this skill, the `fal-agentic-vocabulary` skill, and the files in scope. An auditor that never saw the writing session reads from the position that the future reader will occupy.

The fresh context still needs the change itself: which files and regions it touched, and what it did to them. Step 1 reads the focus of the pass from that description. A diff serves. The reasons, the discussion, the rejected alternatives, and the wording of the instructions do not belong in that description. Passing them rebuilds the position that the fresh context exists to leave.

Losing the session removes one kind of detection and supplies another. Some cues speak of the session, such as an instruction restated or the case that the session raised. Only an auditor who holds that knowledge can check them as written. A fresh auditor holds a counterpart of each: the question of why the artifact says this at all. It arises at a reference that nothing resolves, an element that nothing in scope motivates, and emphasis that the material itself does not explain. Neither position sees every leak, so someone who held the session reviews what the fresh pass reports.

## Run the audit

### 1. Set the scope and the focus

Scope starts with the files that a just-finished change touched or the files that the user names. It follows relevant relationships beyond them as the checks require. Preexisting text is in scope, not only the text that the current change touched.

Within that scope, the regions do not carry equal risk. A change in hand puts the risk in the changed text and in the seams where it meets what was already there. A reading of the change alone misses the seam risk. A defect that the user points to is the starting point of the pass and not its scope, because text that fails one check usually fails others. Take its neighbors with it. With neither a change nor a pointer, the focus rests on the end-to-end read of step 2.

The checks narrow as well, because running every one of them over every region in focus is expensive. Most checks are about the state of the text and do run everywhere in focus. The rest are recognizable by their cues, which speak of something added, removed, or replaced. Each applies where the change did that thing and is set aside where it did not. Without a change in hand, these checks run like the others.

### 2. Read each file end-to-end

Placement and duplication are invisible line by line.

Work file by file, except where a check judges a unit against a set. Those checks say so. Read what they compare against before deciding any one file.

### 3. Run the checks in order

Read the reference file that the scope calls for. When the change touched both media, read both reference files.

Run the checks in the order in which each file lists them. The order is deliberate: an earlier fix tends to remove material that a later check would otherwise have examined.

A check that fires is not yet a finding. The cue is a lead. It marks where to suspect the antipattern that the `fal-agentic-vocabulary` skill defines. Whether the suspicion holds is a judgment, sometimes with nothing in reach to settle it. Where the check names what passes it, judge against that condition first. Checks can also pull against each other over the same text. The tension is in the concerns themselves, and which concern governs the case is the same judgment. Unless the "Authority and reporting" section limits the pass to reporting, apply each check's fix on discovery.

### 4. Revise the audit's own edits

The edits are themselves a change and carry the failures that a change carries, so run the checks over what the audit wrote. The scope of this pass is the audit's own edits, not the files again. One failure is the audit's own doing: preserving the content while keeping the edit small packs an existing unit instead of rebuilding it. This failure is the "under-scoped change" antipattern with the audit's own edit as the change.

## Authority and reporting

This skill fixes the problems that it finds. It deletes, trims, moves, and rewrites on its own judgment. When the request excludes edits, whether by asking for a report, a list, or an evaluation, limit the pass to reporting. When the request says not to change anything, the same limit applies. The words "audit" and "review" do not by themselves impose that limit, because they are what this skill is called.

Report the findings, whether the pass fixed them or proposes the fix. A finding carries four things: where it is, which check fired, what made it fire there, and what the pass did about it or proposes to do. Also state how the pass was narrowed, in scope and in checks, because a pass that narrows silently reads as one that covered everything.

## Editing discipline

- Keep each edit proportional to its purpose, and leave everything that is not being fixed verbatim: same wording, same punctuation, same line breaks, same code. Churn creates diff noise and risks damaging what was fine.
- What is bounded is the reach of the pass, not the size of a fix. Where a check calls for the unit to be rebuilt, rebuild it.
- Prefer, in order: delete, trim, move, rewrite. Deletion is the most effective edit. Rewriting is the easiest to get wrong.
- A restructure preserves behavior unless a check calls for the change, as the "silent fallback" check does.
- A comment-only edit is not always inert. Type-bearing dialects, such as JSDoc types and Python type comments, feed static analysis, and a Markdown edit can break links and anchors. Check what the edit could have broken.

## Japanese

Japanese prose has failure modes beyond what these checks cover. The `fal-write-ja` skill owns the target state, and the `fal-improve-ja` skill holds the audit procedure and examples. When the prose under audit is Japanese, apply them together with this skill.
