# fal-works agent skills

A general-purpose [APM](https://microsoft.github.io/apm/) package for software development and documentation.

## Contents

- [fal-design](.apm/skills/fal-design/SKILL.md): Principles for software design decisions. Counters the tendency to patch rather than redesign.
- [fal-write-docs](.apm/skills/fal-write-docs/SKILL.md): Principles for writing documentation and comments.
- [fal-improve-docs](.apm/skills/fal-improve-docs/SKILL.md): Audit-and-fix pass for existing documentation and comments. Companion to fal-write-docs.
- [fal-write-ja](.apm/skills/fal-write-ja/SKILL.md): Principles for writing natural Japanese, in documentation and elsewhere. Companion to fal-write-docs.
- [fal-improve-ja](.apm/skills/fal-improve-ja/SKILL.md): Audit-and-fix pass for existing Japanese prose. Companion to fal-write-ja.

## Install

```sh
apm install fal-works/skills
# Add --global for user-scoped install
```

To install a single skill:

```sh
apm install fal-works/skills --path .apm/skills/fal-write-docs
```

## Triggering

Each skill's `description` states what the skill is useful for, not when to trigger it. How eagerly a skill should trigger, and in which contexts, varies by user, model, and project. Set those conditions yourself in your `CLAUDE.md` or `AGENTS.md`.

## Design decisions

Structural decisions about this skill collection are recorded as ADRs in [docs/decisions](docs/decisions/).
