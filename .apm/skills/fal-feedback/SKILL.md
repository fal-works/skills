---
name: fal-feedback
description: Write a feedback report on a failure of a fal-* skill, addressed to the maintainer of those skills. Use only when the user explicitly asks for it, typically near the end of a session in which a fal-* skill was applied but the output still fell short. Produces a Markdown report that describes the failure.
---

# Feedback Report

When a `fal-*` skill was in context and the output still fell short, the failure is evidence about the skill. This skill turns that evidence into a report for the maintainer of the `fal-*` skills, who will use it as input when revising them. The whole job is to generate one Markdown file and save it; there is no submission step.

The primary purpose is to describe the failure so that the maintainer can judge it independently. Analysis of causes and proposals for revision are welcome additions, but they stay secondary: a report whose failure description is thin cannot be salvaged by a strong proposal, because the maintainer has nothing to check the proposal against.

## The reader

The reader maintains the `fal-*` skills and knows their texts well. They know nothing else: not this project, not its codebase, not this session, not the conversation that exposed the failure. Two consequences follow.

**Self-contained narration.** The session is the subject matter here, so recounting what happened in it is content, not context leakage. But the account must not presuppose access to anything outside the report. Do not point to the conversation, to project files, or to commits as load-bearing references; restate inside the report whatever the reader needs.

**Generalized background.** The skills apply to any project, so project specifics are noise to their maintainer. Describe the code and the task in terms of roles and structure, at the level of generality where the failure would recur in another project.

Bad (depends on the project): "In `codegen/attrs.py`, the `buildAttributeAccessors` function proposed deleting the `OPTIONAL_WITH_DEFAULT` branch because no attribute in `spec.xml` currently uses it."

Good (generalized, still concrete): "The project generates typed accessors from a specification of elements and attributes. One combination of attribute properties had no occurrences in the current specification, and the agent proposed deleting the branch that handles it."

Generalize the setting, not the evidence. When the failure lives in a specific artifact, such as a wrong name or a wrong sentence, quote that artifact exactly, with its correction where one exists. A failure report abstracted past its own evidence is unverifiable.

## What the report establishes

The material is usually already in the session: the failing outputs, the user's corrections, and the discussion around them. Collect the cases from there before writing.

- **Preconditions.** Which `fal-*` skills were in context, and whether they were read before the failing output was produced. This separates two different diagnoses: a rule that was read but did not act, and a skill that was never loaded. The maintainer cannot recover this information from anywhere else.
- **The failure.** What was attempted, what came out, and what was wrong with it, case by case. Where a correction exists, show before and after. When the user's own words define the judgment, quote them verbatim; a ruling restated in your words loses exactly the nuance the maintainer needs.
- **The skill text held against the failure.** Reread the relevant skill and check each case against its current text. The finding takes one of a few shapes, and naming the shape is most of the analysis: an existing rule already forbids this, a rule covers it but reads too narrowly to fire here, two rules pull in opposite directions on the same fact, or no rule speaks to it. When the current text already forbids the failure, say so plainly; the main cause is then non-compliance, and the maintainer should know that a text revision alone will not fully prevent recurrence. Quote the exact passages, with section numbers, so the maintainer can locate them in the current text.
- **Cause hypotheses, marked as such.** Why the skill failed to prevent the failure is worth conjecturing, but keep the boundary visible between what was observed in the session and what you infer.
- **Proposals, optionally.** Concrete candidate wording helps the maintainer, but frame it as input, not as a patch to apply: the final wording and placement depend on the skills' overall structure, which is the maintainer's call. Proposed fixes to the project itself do not belong in the report at all.

Report the outcome honestly. If the session was corrected into a good final state, say so; if it never converged on a trustworthy result, say that instead. A report that smooths the failure into a tidy lesson is worth less than one that shows how far the skill actually fell short.

## Form

- One Markdown file. Save to the current directory unless the user names a location, under a descriptive kebab-case filename that names the skill concerned and the topic.
- Write in the language of the session unless the user specifies one. Quoted skill text stays in its original language. When the report is Japanese, the fal-japanese skill applies to it.
- The report is documentation, so fal-write-docs applies in full. In particular, let the material shape the structure. A multi-case failure analysis, a report built around one improvement proposal, and a log of many small corrections are all valid shapes. Pick whichever fits what actually happened, and drop any of the elements above that the material does not support rather than padding a section to have it.
