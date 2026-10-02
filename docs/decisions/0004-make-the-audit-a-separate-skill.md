---
status: accepted
created: 2026-10-02
updated: 2026-10-02
---

# Make the audit a separate skill

## Context and Problem Statement

Some skills in this collection define norms for work as it is written. Those norms do not prevent every failure, so the finished work is also audited. The audit has material of its own, such as how to find a failure. The audit examines the work from a different position than the writer does, so that material is kept apart from the norms. Where is that material kept?

## Considered Options

* A separate skill for the audit
* Reference files inside the skill that defines the norms

## Decision Outcome

Chosen option: "A separate skill for the audit", because a skill has a name and a `description` of its own, and reference files have neither.

### Consequences

* Good, because an agent normally has the description of each installed skill in its context from the start of a session, so a description that states what the audit is for keeps the audit visible to the agent.
* Good, because the audit can be invoked by name, by the user or from a user's configuration.
* Neutral, because the description does not determine when the audit runs. Each user sets that.
* Neutral, because most tasks include the audit, so most sessions need both skills. A single skill would also serve those sessions.
* Bad, because the audit skill depends on the definitions in the skill that defines the norms, and cannot be used alone.
