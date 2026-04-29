---
name: improve-comments
description: Use this skill to audit and improve source code comments. Trigger proactively when implementing or reviewing a code change that adds or modifies a substantial number of comments. Comment quality degrades silently in such changes. Running an audit pass before finalizing keeps low-quality comments from landing. Applies to any language and any doc-comment dialect (like C-style block, line comments, Python triple-quoted, JSDoc, Pydoc, Rustdoc).
---

# Improve Comments

Source code comments are not free decoration. Every comment occupies reader attention and ages alongside the code it sits next to. A comment is worth keeping only when it carries information the code itself cannot express.

This skill is a structured audit-and-fix pass. Default to subtraction. Removing dead-weight comments makes the surviving ones louder.

## Operating principle

A comment earns its place when it does one of these:

1. **Contract** — describes what a function, type, or module guarantees to callers, at the right abstraction level. Not narrower than the code is general; not more granular than the reader needs.
2. **Why, not what** — captures a non-obvious reason: a workaround, a subtle invariant, surprising behavior, a hidden constraint, a chosen trade-off.
3. **Existence justification** — explains why a thing exists when its purpose is not obvious from name and signature alone.

Anything else is a candidate for deletion or rewrite.

## The audit checklist

Read the target file end-to-end. For each comment, walk the checks below in order. They are arranged from most fundamental ("does this comment have a reason to exist?") to most escalatory ("is this really a code-structure problem in disguise?"). One comment can hit several; the categories overlap on purpose so a problem caught by any of them gets caught.

1. The code already says this
2. The comment lies
3. The comment leaks context the future reader does not have
4. The doc describes the caller's use case instead of the callee's contract
5. Wrong location
6. Wrong abstraction level
7. Verbose construction
8. The comment is masking a code-structure problem

### 1. The code already says this

The comment carries no information beyond what the code itself expresses. Two surface forms:

- **Function or type doc that paraphrases the implementation.** "Calls `parse` then `validate` and ...", "iterates the array and ...". The doc rots when the body is rewritten, and worse, callers get a mechanism description instead of the contract they need.
- **Inline comment that narrates a self-evident statement.** A line above `i++` that says "increment counter".
- **Type doc that restates the type expression.** Prose like "field is required" or "X is optional" when the type already encodes that fact.

Both fail the same test: the reader gains nothing from reading the comment that they would not get from reading the code.

**Bad** (doc level):
```
/// Iterates over `users` and returns a list of their email addresses.
fn user_emails(users) -> [string]
```

**Good** (doc level):
```
/// Return each user's email address.
fn user_emails(users) -> [string]
```

**Bad** (inline level):
```
// increment counter
i += 1
```

**Good** (inline level):
```
i += 1
```

**Fix**: at the doc level, rewrite at the contract level (what callers can rely on); let the body show how. At the inline level, delete and trust the names. If the names are bad, fix the names.

**Note**: implementation details belong in a doc when callers depend on them. The canonical case is complexity: "binary-search" tells callers `O(log n)`, which is part of the contract. The test is not whether the detail appears in the body; it is whether a caller can rely on it.

### 2. The comment lies

The comment says X but the code does Y. Usually the result of code changing while the comment stayed put. These are worse than no comment at all because they actively corrupt the reader's mental model.

**Fix**: decide which one is correct. If the code is right, fix or delete the comment. If the comment captures the intent and the code drifted, that is a code bug. Surface it to the user; do not silently rewrite the comment to match a buggy implementation.

### 3. The comment leaks context the future reader does not have

The comment makes sense only to someone who shared the moment of writing — the writer's instructions, the discussion that led to the design, the previous version of the code. A future reader (or the same person months later) cannot access that context. The comment becomes a riddle, then a misleading riddle, then it rots.

This pattern is especially common in LLM-assisted coding because the writing session is full of context (specs, agreed decisions, alternatives considered) that quietly leaks into the output.

Common forms:

- **Change history.** "previously did X", "new in v2", "replaced Y", "old: ... / now: ...". Code describes the *current* state. History belongs in version control.
- **Session or instruction provenance.** "Per spec §3.2:", "As agreed:", "Following the design discussion:". The reader does not know which spec, which agreement, which discussion.
- **Unprompted rationale.** "X is always present because…", "Y is nullable here because…". The same justification is load-bearing where the property would surprise a fresh reader, and noise where the reader takes it for granted. The leaked context is whatever made the writer feel the explanation was needed — usually a recent decision or discussion that made the question feel live. To a future reader who never asked, the rationale only raises a different question: "why is this being explained?"
- **References to files outside the tracked repository.** "See `notes.md`", "Described in `design.txt`". If the file is gitignored, build-generated, or only on the writer's machine, a fresh clone cannot follow the pointer. A common LLM-coding variant: a local scratch file (session plan, working note) gets cited as if it were a permanent project document.
- **References to a road not taken.** "Doing B, not A" written purely because the writer was told to do B instead of A. The reader has no idea what A was or why it matters.

**Important nuance for the last form**: "Doing B, not A" is *useful* when A is what a typical reader would expect and B is the surprising choice — least-surprise documentation is contract-bearing. The line to draw: **does the future reader, with no access to the writer's context, benefit from knowing about A?** If yes, keep and explain why B was chosen. If the only reason A is mentioned is that the writer was instructed to deviate from it, delete.

**Bad**:
```
// Per spec §3.2: extract first line, truncate if too long.
// (As agreed in the design discussion.)
fn format_slice(text) -> string
```

**Good**:
```
/// Extract the first line and truncate if too long.
fn format_slice(text) -> string
```

**Fix**: delete. If the *current* code has a non-obvious property (e.g., "this looks like a no-op but is load-bearing because callers depend on the side effect"), state that property directly without referencing the change or the conversation that produced it.

### 4. The doc describes the caller's use case instead of the callee's contract

A function (or type, or module) is more general than its doc admits. The caller's domain, intent, or precondition has slipped into the doc *as if it were a property of the function itself*. A future caller looking for the right helper may pass it over, and the moment a second caller arrives in a different domain, the doc is actively misleading.

Like check 3, this is a context leak — the writer was thinking about the immediate calling site while writing — but the leaked content is the *caller's perspective* rather than session or instruction provenance.

Common forms:

- **Domain narrowing.** `average(xs)` documented as "calculates the average of monetary amounts" when the function works for any numeric list.
- **Use-case narrowing.** `parseRange(s)` documented as "parses the page-range input from the print dialog" when the function is a general range parser.
- **Caller-tied invariants.** Param `text` documented as "the user's bio text" when the function accepts any string.
- **Class or module scope tied to the current consumer.** `class Cache<K, V>` documented as "stores user sessions" because that is the only consumer today.

**Bad**:
```
/// Average price of the items in the user's cart.
fn average(xs: [Number]) -> Number
```

**Good**:
```
/// Arithmetic mean. Empty input returns 0.
fn average(xs: [Number]) -> Number
```

**Important nuance**: caller-side facts are not absolutely banned from a doc. The failure is presenting them *as if they were intrinsic to the function*. A doc that names the relationship explicitly — "Used for X", "Used by Y", "Assumes the input is non-empty because callers check first" — is honest about the asymmetry, and is acceptable when the code itself ought to be reshaped (a narrower function, a stronger type, a relocated helper) but that work is being deferred. Treat such explicit framing as a softer signal, akin to check 8: the doc is doing its job, but the structure may want revisiting.

**Fix**: rewrite at the function's actual level of generality. If the function should *only* be used for one domain, that is a code-level decision — narrow the type, narrow the name, or move it next to its caller — not something the doc should silently impose.

### 5. Wrong location

The comment exists for a real reason but sits in the wrong place. Two surface forms:

- **Duplicated content across layers.** The same statement appears at a contractual source (a type doc, a callee's doc, a module-level constant) and is restated at a downstream site that uses it. A common case: a caller has a comment about what its callee returns, even though the callee's doc already says so. Whichever copy drifts first becomes inconsistent.
- **Type or class behavior described on every user instead of on the type itself.** Each callsite repeats what the type guarantees.

Both share the same root: **knowledge belongs at the most contractual surface**. Repetition at downstream sites pays the prose cost N times and creates N drift points.

**Bad**:
```
fn compare(a: User, b: User) {
  // Users have a `priority` field used for sorting, lower numbers first.
  ...
}
fn pick_winner(users: [User]) {
  // Users have a `priority` field used for sorting, lower numbers first.
  ...
}
```

**Good**: put the priority-field documentation on the `User` type itself; the operating functions reference it by name.

**Fix**: hoist the statement to the most contractual surface that owns it (the type, the module, the function header). Drop the downstream copies.

### 6. Wrong abstraction level

The comment lists specific identifiers, fields, branches, or cases when a single abstract sentence would carry the same content. The list pads volume without adding signal, and breaks when an item renames or moves.

**Bad**:
```
/// Per-diagnostic shape after schema validation.
/// Only fields the wrapper consumes are kept; unused source fields
/// (`severity`, `causes`, `url`, `help`, `related`, label text)
/// are not retained.
```

**Good**:
```
/// Per-diagnostic shape after schema validation. Only fields the wrapper
/// consumes are kept.
```

**Fix**: move up one level of abstraction. Drop the enumeration. If a particular item carries non-obvious meaning of its own, comment it inline at its own location, not in this summary.

### 7. Verbose construction

One sentence that should be three. Heavy use of colons, semicolons, em-dashes, or parentheticals to chain clauses. The reader must parse the connector before extracting the meaning.

**Bad**:
```
/// `schemaMismatch` is non-null when the captured stdout parsed as JSON but its
/// shape diverges from what the wrapper consumes; this signals a contract
/// mismatch — typically a schema bump on a caret-pinned dep, or a structured
/// fatal payload — and `reason` describes which field failed, while
/// `formattedStdout` then carries the raw stdout for relay.
```

**Good**:
```
/// `schemaMismatch` is non-null when the captured stdout parsed as JSON
/// but its shape diverges from the wrapper's contract.
///
/// `reason` names the offending field.
/// In that case `formattedStdout` carries the raw stdout for relay.
```

**Fix**: split into short declarative sentences (and paragraphs, too). One thought per sentence. Treat connectors (especially em-dashes) as expensive.

### 8. The comment is masking a code-structure problem

The code is poorly factored, and the comment apologizes for it. Two layers of low quality compound: the code is hard to read AND the prose compensating for it is hard to read. The reader pays twice.

**Bad**:
```
fn process(d) {
  // Normalize the legacy form into the new form: legacy entries have `name`
  // but new entries have `id`, so we copy `name` into `id` if missing.
  if (!d.id) { d.id = d.name }
  // Then validate.
  ...
}
```

**Good**:
```
fn process(d) {
  d = migrate_legacy_to_id(d)
  validate(d)
  ...
}
```

**Fix**: restructure the code so naming and structure carry the meaning. After the restructure, the apology comment is usually unnecessary. This is more work than editing prose. **Always surface the option to the user before doing it**; do not silently expand a comment task into a refactor.

## The inverse: comments that should exist but do not

While auditing, also watch for places where a missing **why** comment would help. Anywhere the code does something non-obvious — a workaround for a specific bug, a subtle invariant the rest of the code depends on, surprising behavior that would trip a reader, a hidden constraint from the environment — deserves a brief why-comment.

The audit is not only subtractive. Adjusting comment density toward the places that need it is part of the job.

## Workflow

### Step 1 — Read

Read the target file in full. Build a mental map of every comment, what it is doing, and which checks it fails.

Comments you wrote in the same session should be audited more skeptically than older ones.

### Step 2 — Plan and align with the user

For each problem area, draft a one-line proposal: **delete / trim / move / rewrite / restructure-code**, with a short reason that names the check.

Format the plan as a list grouped by location:

```
1. <symbol or line range>: <action> — <reason>
2. ...
```

Also list "leave as is" items briefly so the user sees what is being preserved.

**Present the plan to the user before editing.** Comment quality is partly a matter of taste, and the user may want to keep specific comments you would cut, or vice versa. Skipping this step risks overshoot (deleting comments the user values) or undershoot (trimming around comments that should disappear). Confirmation is cheap; reversing edits is not.

**When no user is reachable** (running unattended, in CI, in an autonomous agent loop): still emit the plan as a visible artifact before the edits — in your output, the commit message, or the PR description — so a reviewer can verify the choices after the fact. Plan-then-edit ordering is not skippable. It applies to both the action sequence and the written report: present the plan first, then the edits-applied summary. Only the wait-for-confirmation step changes shape.

For check 2 (a lying comment whose code is buggy) and check 8 (an apology comment masking a code-structure problem), do not act unilaterally: leave those cases untouched in the file and surface them as separate items for review. "Untouched" extends to any adjacent prose that participates in the same conflict. A docstring or header comment that asserts the same false claim as a flagged inline comment stays as-is. Rewriting it would silently take a side.

### Step 3 — Edit

After sign-off, apply the changes. Preference order when in doubt:

1. **Delete** — first choice. Removing dead weight is the highest-leverage edit.
2. **Trim** — second choice. Shorter prose with the same content.
3. **Move** — when the content is good but in the wrong location (check 5).
4. **Rewrite** — sparingly. Often a sign that the original was right but verbose, or right but at the wrong abstraction.
5. **Restructure code** — only when check 8 applies and the user has agreed.

**Preserve means verbatim.** A comment that clears the audit stays as it was. Same wording, same punctuation, same line breaks. Cosmetic touches are out of scope. They create churn without information gain, and risk turning a survivor into a casualty over multiple audit passes.

When writing replacement prose, do not introduce new long sentences while removing old ones. Apply the same style discipline you are enforcing.

### Step 4 — Verify

If the project has lint, test, or type-check commands, run them. Comment-only changes should not affect runtime, but type-bearing comment dialects (JSDoc-like type annotations, Python type comments, Sphinx directives) can affect static analysis. Skipping this step is how a "harmless" comment edit breaks a type check.

## Style notes for replacement prose

- Default to no comment. Add only when it earns its keep.
- One thought per sentence. Split before chaining.
- Avoid em-dashes as connectors. Prefer a period (split) or parentheses (subordinate).
- Do not refer to the current task, fix, or conversation ("added for X", "as discussed"). That is the context-leak failure of check 3.
- Do not let a caller's domain or intent shape the contract sentence as if it were the function's own (check 4). Where it must appear, frame the relationship explicitly — "used for X", "assumes Z" — rather than absorbing it.
- Match the voice and register of the surrounding file. A terse file stays terse. A descriptive file stays descriptive.
- Do not restate the function's name in its doc summary.
- Avoid breaking lines mid-phrase. Wrap at a clause boundary, or leave the sentence on one line. Prefer starting each sentence on its own line, even when the previous one left room.
