# Authoring the agentic skills

This document states the rules for changing the content and the structure of the `fal-agentic-vocabulary` and `fal-agentic-audit` skills.

## Evaluating changes

A change to the skills is evaluated by reasoning against the model and the rules in this directory. Measuring the effect of the skills is not used as a method of evaluation. The situations in which the skills are used are too varied and complex for a measurement in any particular setting to give a meaningful result.

The reader of a skill is an LLM working in a demanding context. The context can be long, hold many concerns, or have been compressed. A rule can therefore go unapplied at the moment of judgment even when it can be derived from elsewhere in the skill. For this reason, that a sentence can be derived from elsewhere in the skill is not by itself a reason to remove it.

## What each skill contains

What counts as a failure belongs to the vocabulary skill. How to find a failure and how to weight it belong to the audit skill.

Which bias generates which antipattern is recorded only in the model.

The vocabulary skill contains no concrete examples. A concrete example is an actual phrase, file, or piece of code. Naming a kind of form is not a concrete example. There are two reasons for excluding them. An example of a failure induces the failure. An example of any kind also narrows the concept to the example's form, and in a demanding context the reader retains the example after losing the definition.

The vocabulary skill refers to a similar node only in two cases. One is where the reference is needed to explain what the node takes as the problem. The other is where the reference prevents the failure of noticing one node and missing the other. A passage whose main purpose is to show how to tell the nodes apart is not written. Without this limit, a reference to a node is added whenever some relation to it exists, and the text grows without need.

Neither skill contains instructions for verification methods that differ by project, such as linting, tests, and builds. A check that applies to any project can be included.

## Structure of the vocabulary skill

The structure of the vocabulary skill is derived from the model as follows, and no other basis for ordering or grouping is added.

### Sections and groups

- The sections are grouped by the value of `looks_mainly_at`, and the groups are ordered within, above, beside, and outside.
- Each group heading is a short phrase that states where the principles in the group look.

### Order of sections

The order is decided in two stages:

1. Specialization and composition constrain the order of the principles in `model.yaml`. The specializations of a principle follow it as a block. The same applies where a specialization is itself specialized. A composite principle follows those of its components that are in the same group, and it is usually the last principle in its group.
2. The order of the sections in the skill text follows the order in `model.yaml`.

### Content of a section

The paragraphs of a section are in this order:

1. The description of the principle. It can span several paragraphs. A paragraph that determines what the test judges belongs to the description.
2. The test, if any.
3. The prose on applying the principle.
4. The antipattern definitions. A parent antipattern precedes its specializations.
5. The paragraph on overapplying the principle, if any.

A principle can have more than one test.

An antipattern definition is one paragraph. The paragraph contains only the definition, together with any remedy that accompanies it, and begins with a sentence that introduces the name. Content about the antipattern that does not belong in its definition is placed in the prose on applying the principle. That prose precedes the definitions, so it does not use the antipattern's name.

## Structure of the audit skill's reference files

The audit skill has one reference file per medium, `code.md` and `documents.md`. Each file has two sections, "Cues" and "Judgment notes."

- Neither file refers to the other, so that the auditor of one medium reads nothing about the other.
- The cues of each medium are written for that medium and cover every antipattern that appears in it. An antipattern is left out of one medium's cues only when the concept is specific to the other medium.
- A cue is placed in a category by how it is found, not by the kind of thing observed. A feature found only by questioning a unit, or by comparing it with something outside the passage, goes in the last category, "Units questioned or compared," even when it concerns a name or a phrase. The other categories are consulted when a feature is noticed during reading, so a cue that reading alone does not reveal would be skipped there.
- A cue or a judgment note that has a clear counterpart in the other file has the same relative order in both files. Where the correspondence is ambiguous, no order is required. A cue or a judgment note that only one file has is placed where it fits best in that file.
