# Agent instructions

This repository is an [APM](https://microsoft.github.io/apm/) package of agent skills for software development and documentation.

## Rules for every skill

The source of each skill is in `.apm/skills/`. For work on a skill, read the source and edit the source. A session can load a copy of a skill that is installed in the agent's environment. That copy can differ from the source, and a difference between them is not an error.

The skills in this repository apply to work in this repository as they apply to work elsewhere. For example, the `fal-agentic-vocabulary` skill covers what work in this repository produces, such as a proposal to the user and the text of a skill. Load each skill that applies to the task, and follow it. Where the loaded copy differs from the source, following either of them is sufficient.

A skill's `description` only needs to state what the skill is useful for. Whether the description triggers the skill reliably is not a concern when improving a skill. The conditions for triggering vary by user, model, and project, so each user sets them in their own configuration.

## Rules in `docs/agentic-vocabulary/`

The rules in `docs/agentic-vocabulary/` apply to the following work:

- A change to, or a review of, the `fal-agentic-vocabulary` skill or the `fal-agentic-audit` skill
- Writing or changing a reference, in any other skill, to a name that the `fal-agentic-vocabulary` skill defines, such as the name of a principle or an antipattern

The README of that directory states which documents to read.
