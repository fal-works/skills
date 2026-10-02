# Agent instructions

This repository is an [APM](https://microsoft.github.io/apm/) package of agent skills for software development and documentation.

## Rules for every skill

The source of each skill is in `.apm/skills/`. A session can load a copy of a skill that is installed in the agent's environment, and that copy can differ from the source. For work on a skill, read the source and edit the source.

A skill's `description` only needs to state what the skill is useful for. Whether the description triggers the skill reliably is not a concern when improving a skill. The conditions for triggering vary by user, model, and project, so each user sets them in their own configuration.

## Rules in `docs/agentic-vocabulary/`

The rules in `docs/agentic-vocabulary/` apply to the following work:

- A change to, or a review of, the `fal-agentic-vocabulary` skill or the `fal-agentic-audit` skill
- Writing or changing a reference, in any other skill, to a name that the `fal-agentic-vocabulary` skill defines, such as the name of a principle or an antipattern

The README of that directory states which documents to read.
