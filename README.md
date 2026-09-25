# fal-works agent skills

A general-purpose [APM](https://microsoft.github.io/apm/) package for software development and documentation.

## Contents

- [fal-agentic-vocabulary](.apm/skills/fal-agentic-vocabulary/SKILL.md): Named principles and antipatterns for the artifacts of agent work, such as documentation, comments, code, and software design decisions.
- [fal-agentic-audit](.apm/skills/fal-agentic-audit/SKILL.md): Audit-and-fix pass for a finished change, covering documentation, comments, and code structure. Companion to fal-agentic-vocabulary.
- [fal-write-ja](.apm/skills/fal-write-ja/SKILL.md): Principles for writing high-quality Japanese, in documentation and elsewhere. Companion to fal-agentic-vocabulary.
- [fal-improve-ja](.apm/skills/fal-improve-ja/SKILL.md): Audit-and-fix pass for existing Japanese prose. Companion to fal-write-ja.
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

## Design decisions

Structural decisions about this skill collection are recorded as ADRs in [docs/decisions](docs/decisions/).
