# fal-works agent skills

A general-purpose [APM](https://microsoft.github.io/apm/) package for software development and documentation.

## Contents

- [fal-agentic-vocabulary](.apm/skills/fal-agentic-vocabulary/SKILL.md): Named principles and antipatterns for the artifacts of agent work, such as documentation, comments, code, and software design decisions.
- [fal-agentic-audit](.apm/skills/fal-agentic-audit/SKILL.md): Audit-and-fix pass for a finished change, covering documentation, comments, and code structure. Companion to fal-agentic-vocabulary.
- [fal-write-ja](.apm/skills/fal-write-ja/SKILL.md): Principles for writing high-quality Japanese, in documentation and elsewhere. Companion to fal-agentic-vocabulary.
- [fal-improve-ja](.apm/skills/fal-improve-ja/SKILL.md): Audit-and-fix pass for existing Japanese prose. Companion to fal-write-ja.
- [fal-itemized-human-review](.apm/skills/fal-itemized-human-review/SKILL.md): Temporary HTML page on which the user reviews a text artifact item by item, with a decision and a note for each item.
- [fal-feedback](.apm/skills/fal-feedback/SKILL.md): Report a failure of another `fal-*` skill, for use as input when revising it.

## Install

```sh
apm install fal-works/skills
# Add --global for user-scoped install
```

To install a single skill:

```sh
apm install fal-works/skills --path .apm/skills/fal-agentic-vocabulary
```

## Triggering

Each skill's `description` states what the skill is useful for, not when to trigger it. How eagerly a skill should trigger, and in which contexts, varies by user, model, and project. Set those conditions yourself in your `CLAUDE.md` or `AGENTS.md`.

The audit skills are an example. If you want finished work to be audited, state when. For example:

```markdown
Before reporting a change as complete, apply the `fal-agentic-audit` skill to it.
```

## Design documents

- [docs/decisions](docs/decisions/): ADRs that record structural decisions about this skill collection.
- [docs/agentic-vocabulary](docs/agentic-vocabulary/): The model that `fal-agentic-vocabulary` and `fal-agentic-audit` are based on, and the rules for editing those skills.
